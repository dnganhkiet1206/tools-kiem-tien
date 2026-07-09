# YAAS RELEASE ENGINEERING SPECIFICATION (Quality Gate System)

```yaml
version: 1.0.0
last_updated: 2026-07-09
status: BLUEPRINT            # chưa hiện thực — file này là bản thiết kế để implement, không sửa repo hiện có
input: AUDIT-2026-07-09.md   # mọi gate truy nguyên về một failure class có thật trong audit
owner_layer: governance      # thay đổi file này = thay đổi kiến trúc, theo luật GOVERNANCE
```

## 0. Nguyên tắc thiết kế (rút từ nguyên nhân gốc F3 của audit)

1. **Enforcement over Declaration:** một luật chỉ được coi là tồn tại khi có check máy tương ứng. Luật chưa check được → ghi vào sổ `WAIVERS` có hạn, không được giả vờ đang hiệu lực.
2. **Fail-closed cho tầng canonical, fail-open-có-cảnh-báo cho tầng nội dung:** schemas/exports/governance hỏng → chặn merge tuyệt đối; prose/citation nghi vấn → warning + yêu cầu người xác nhận. Lý do: false positive ở tầng nội dung sẽ dạy operator thói quen bỏ qua gate — cái chết của mọi hệ QA.
3. **Tôn trọng M-tiers:** gate đọc `required_from` trong schema; không đòi M3-compliance ở module M1 (chống-quan-liêu i1 của GOVERNANCE áp cả vào QA).
4. **Nhanh ở local, kỹ ở CI:** pre-commit <5 giây (chỉ check file đổi); CI đầy đủ <2 phút toàn repo. Gate chậm = gate bị skip.
5. **Output máy-đọc:** mọi check xuất JSON (`{check_id, severity, file, line, message, waiver_id?}`) — để AI agent tự sửa được lỗi gate báo, và để thống kê KPI gate.
6. **Gate cũng có thể sai:** hệ có ngân sách false-positive, quy trình waiver, và KPI riêng (§10). Gate không được trở thành lý do repo ngừng tiến hoá.

## 1. Kiến trúc 3 tầng gate

```
L0  PRE-COMMIT (git hook, local, <5s, chỉ file thay đổi)
    → chặn: YAML parse fail, secret, key máy không-phải-tiếng-Anh, link nội bộ gãy trong file đổi
L1  PULL REQUEST CI (toàn repo, <2 phút, bắt buộc xanh mới merge)
    → toàn bộ 20 nhóm check §3; comment kết quả JSON+bảng vào PR
L2  RELEASE GATE (thủ công kích hoạt khi tag phiên bản repo)
    → L1 + link ngoài + golden tests prompt + đối chiếu CHANGELOG + checklist người ký
NGOÀI CHU KỲ: CRON THÁNG — staleness (future_review quá hạn), link ngoài, tool `evaluated_at` quá 2 quý
    → không chặn gì, chỉ mở issue tự động (fail-open: repo bỏ hoang thì issue là bằng chứng mục nát)
```

Bộ máy: một CLI duy nhất `tools/spec_lint.py` (Python stdlib + `pyyaml` + `jsonschema`; không SaaS, chạy được offline trên laptop — đúng ràng buộc solo operator). Pre-commit, CI, cron đều gọi CLI này với profile khác nhau (`--profile l0|l1|l2|cron`). GitHub Actions chỉ là kẻ gọi lệnh; đổi CI vendor không đổi gate (chống vendor lock-in cho chính hệ QA).

## 2. Truy nguyên: gate nào chặn failure class nào của audit

| Failure class (audit) | Gate chặn | Tầng |
|---|---|---|
| F1 schema không parse | CHK-01/02/03 | L0+L1 |
| F2 canonical sai (key VI, target giả, unreachable) | CHK-04/05 | L0 (key) + L1 (graph) |
| F2b prose↔export lệch | CHK-06 (generated-block hash) | L1 |
| F3 DoD không chạy | toàn hệ này + CHK-10 biên bản DoD | L1 |
| F4 state machine mơ hồ | CHK-05 (policy-vs-state, terminal, gate flags) | L1 |
| F6 secret/license | CHK-12/13 | L0 (secret) + L1 |
| F7 hai nguồn status | CHK-07 (single-source rules) | L1 |
| F9 dangling refs, citation thiếu ngày | CHK-07/11/14 | L1 |
| Breaking change không khai báo | CHK-16 (semver diff) | L1 |
| Prompt schema trôi/vendor-specific | CHK-09 | L1 |

Luật bảo trì: **mỗi failure class mới phát hiện trong tương lai phải sinh ra một CHK mới hoặc mở rộng CHK cũ trong cùng PR sửa lỗi** — hệ gate lớn lên bằng chính lỗi thật, không lớn bằng tưởng tượng.

## 3. Danh mục check (CHK-*) — hợp đồng hành vi, không phải code

**CHK-01 YAML Validation** — `yaml.safe_load` mọi `*.yaml|*.yml`; cấm tab indent, cấm duplicate key (loader strict). Severity: BLOCK. *(Chặn F1.)*

**CHK-02 JSON Schema meta-validation** — mọi file trong `schemas/` phải là JSON Schema draft 2020-12 hợp lệ (validate bằng metaschema). Điều kiện tiên quyết: đợt sửa B1 chuyển schema tự chế → JSON Schema; gate này khoá không cho tái phát minh cú pháp riêng. BLOCK.

**CHK-03 Instance validation** — mọi tài liệu có cấu trúc validate đúng schema của nó: module header ↔ `module.schema`, bản ghi tool trong `exports/tools.yaml` ↔ `tool.schema`, prompt ↔ `prompt.schema`, state machine ↔ `statemachine.schema` (schema mới, phải viết trong đợt implement). BLOCK.

**CHK-04 Machine-key language rule** — mọi key và enum-value trong `schemas/` + `exports/`: `^[a-z][a-z0-9_]*$`, đối chiếu wordlist tiếng Việt-không-dấu để bắt `du/thieu/sach/loi` dạng lọt regex. Prose tiếng Việt tự do. BLOCK ở exports/schemas, WARN ở nơi khác. *(Chặn F2-ngôn-ngữ; luật hoá A6.)*

**CHK-05 State machine graph validation** — parse `exports/state-machine.yaml`: (a) mọi transition target tồn tại trong `states` hoặc là policy được khai trong `globals`; (b) mọi state reachable từ state khởi đầu; (c) ≥1 terminal state, mọi terminal khai `next: null` tường minh; (d) states gắn `human_gate: true` phải khớp danh sách `globals.human_gates` — **xoá human gate = gate đỏ** (bảo vệ ràng buộc đạo đức/chính sách bằng máy); (e) không state nào trùng tên field chuẩn (`validation`, `entry`...). BLOCK. *(Chặn F2/F4.)*

**CHK-06 Generated-content drift** — bảng prose + Mermaid của state machine phải nằm giữa marker `<!-- GENERATED FROM exports/state-machine.yaml sha256:… -->`; check tính lại hash nguồn và so. Lệch = BLOCK kèm lệnh sửa (`python tools/gen_views.py`). Tổng quát hoá cho mọi cặp nguồn→view sau này. *(Diệt gốc "3 biểu diễn lệch nhau".)*

**CHK-07 Cross-reference & single-source** — mọi link nội bộ `[.](path)` trỏ tới file/anchor tồn tại; mọi thư mục/file được nhắc trong prose (`prompts/`, `tools/`) phải tồn tại hoặc mang tag `(planned)`; status module chỉ được xuất hiện ở README (regex bắt bảng status lạc chỗ). BLOCK link gãy, WARN single-source. *(Chặn F7/F9.)*

**CHK-08 Governance validation** — GOVERNANCE version tăng nếu diff chạm `schemas/` hoặc các mục PHASE 5–7; mọi luật mới trong GOVERNANCE phải khai `enforced_by: CHK-xx | WAIVER-xx` (nguyên tắc §0.1 tự áp lên chính governance); file WAIVERS: mỗi waiver có hạn, quá hạn = BLOCK.

**CHK-09 Prompt validation** — đủ 12 khối đúng thứ tự; blacklist token vendor-specific (tên tính năng riêng của từng vendor, cập nhật được qua config); prompt có `status: production` bắt buộc có `golden_tests` (≥1 cặp input/expected + tiêu chí chấm) và `model_tested` không rỗng. BLOCK cho production prompt, WARN cho draft.

**CHK-10 Documentation/DoD validation** — module status ✅ trong README phải có: header đủ trường theo tier, Context Capsule ≤15 dòng, Self Audit đủ 8 câu và có ≥1 điểm yếu (heuristic: mục self-audit không được chứa toàn "không"), file biên bản DoD trong `records/dod/NN.md` có ngày + người/agent chạy. BLOCK khi claim ✅ mà thiếu; module 🔜 không bị đòi.

**CHK-11 Evidence validation** — dòng chứa claim định lượng (heuristic: số + đơn vị %/GB/tok/s/$/điểm) trong module phải có nhãn `[A]|[B]|[C]|VERIFY|INSUFFICIENT EVIDENCE` trong cùng đoạn; citation ngoài phải kèm ngày truy cập `(truy cập YYYY-MM)`. WARN (không BLOCK — heuristic có false positive; người xác nhận từng cảnh báo trong PR checklist). Cron: nhãn VERIFY tồn tại quá 2 chu kỳ quý → issue.

**CHK-12 Secret detection** — regex bộ mẫu (Google API key `AIza…`, OAuth client secret, bearer token, private key block, chuỗi entropy cao trong file config) trên diff (L0) và toàn repo (L1); baseline file cho false positive đã duyệt. BLOCK. Kèm luật: mọi credential chỉ nằm trong `.env` (gitignored) — luật này ghi vào GOVERNANCE trong đợt B5.

**CHK-13 License validation** — `LICENSE` tồn tại ở root repo; mọi bản ghi tool có field `license` không rỗng và thuộc danh sách SPDX hoặc mang `VERIFY`; tool được dùng trong pipeline production (recommended_stage ≠ optional) không được có license `non-commercial`. BLOCK thiếu LICENSE/field; WARN cho VERIFY.

**CHK-14 Link validation (external)** — HEAD request các URL ngoài; chỉ chạy L2 + cron (không chạy PR — mạng chập chờn không được quyền chặn merge). Chết 2 lần liên tiếp → issue gợi ý archive.org snapshot. WARN-only.

**CHK-15 Mermaid validation** — parse bằng `mmdc`/mermaid parser nếu môi trường có node; không có → SKIP có ghi nhận (không giả vờ đã check). Vì Mermaid là view sinh từ YAML (CHK-06), rủi ro thật đã được chặn ở nguồn; check này là phòng tuyến phụ. WARN.

**CHK-16 Breaking-change detection** — diff cấu trúc `schemas/*` và `exports/state-machine.yaml` giữa base↔head: xoá field/state/transition, đổi type, thêm required → đòi (a) bump major/minor đúng semver trong file, (b) mục CHANGELOG.md tương ứng, (c) label `breaking` trên PR. BLOCK nếu thiếu bất kỳ. Chính sách tương thích: linter chấp nhận schema_version N và N−1 (từ audit §8), quá N−1 = BLOCK.

## 4. Validation Pipeline (thứ tự chạy trong `spec_lint.py`)

```
parse (01) → meta-schema (02) → instances (03) → language (04) → graph (05) → drift (06)
→ xref (07) → governance (08) → prompts (09) → docs/DoD (10) → evidence (11)
→ secrets (12) → license (13) → [L2/cron: links (14)] → mermaid (15) → breaking (16)
Nguyên tắc: fail sớm ở tầng parse thì các check sau SKIP (đừng dội 50 lỗi hệ quả lên 1 lỗi gốc);
mã thoát: 0 = xanh, 1 = có BLOCK, 2 = chỉ WARN (CI pass với cờ --warn-ok ở L1, không cờ ở L2).
```

## 5. CI/CD Workflow (GitHub Actions — phác thảo hợp đồng, code viết ở đợt implement)

```yaml
# .github/workflows/quality-gate.yml
on: {pull_request: {branches: [main]}, push: {branches: [main]}}
jobs:
  gate:
    runs-on: ubuntu-latest
    steps:
      - checkout (fetch-depth: 0)            # cần base để CHK-16 diff
      - setup-python 3.11 + pip install pyyaml jsonschema
      - run: python tools/spec_lint.py --profile l1 --output json > lint.json
      - post lint.json dưới dạng PR comment (bảng + JSON thu gọn)
      - fail job nếu exit code 1
  cron-staleness:                            # file workflow riêng, schedule: monthly
    - run: python tools/spec_lint.py --profile cron
    - mở/ cập nhật issue "Staleness report YYYY-MM" từ output
```

Branch protection: `gate` là required check; không ai (kể cả owner, kể cả AI agent) merge khi đỏ; không có đường "admin bypass" — bypass hợp lệ duy nhất là WAIVER có hạn được commit công khai.

## 6. Git hooks (local)

`pre-commit` framework, config `.pre-commit-config.yaml`: gọi `spec_lint.py --profile l0 --changed-only`. Cài bằng 1 lệnh trong setup.sh. Hook local là tiện ích, KHÔNG phải phòng tuyến (dev có thể `--no-verify`) — phòng tuyến thật là L1; spec ghi rõ để không ai ảo tưởng hook local đủ.

## 7. Pull Request Template (`.github/pull_request_template.md` — nội dung hợp đồng)

- **Loại thay đổi:** [ ] module mới · [ ] sửa module · [ ] schemas/exports (canonical) · [ ] governance · [ ] tools/gate
- **Tự khai:** [ ] chạy `spec_lint.py --profile l1` local, đính kèm tóm tắt · [ ] mọi WARN evidence đã được người xác nhận từng dòng · [ ] không đổi canonical, HOẶC đã bump version + CHANGELOG + label `breaking`
- **Nếu PR do AI sinh:** model + phiên/prompt sinh ra PR; xác nhận không chạm `GOVERNANCE.md`/`schemas/` (hai tầng chỉ người merge — luật từ audit §12)
- **DoD:** module claim ✅ phải link biên bản `records/dod/`

## 8. Review Checklist (cho người/agent review — chỉ những gì máy KHÔNG kiểm được)

1. Nội dung đúng domain không (kỹ thuật, chính sách YouTube, kinh doanh)?
2. Nhãn evidence có bị *gắn sai cấp* không (nguồn C dán nhãn A)?
3. Decision tree có nhánh nào chết/không ai đi tới không?
4. Self Audit có thật không hay viết chiếu lệ?
5. Thay đổi này có làm luật nào trở nên không-enforce-được mà thiếu WAIVER không?
6. Ngôn ngữ prose có giữ được chuẩn "hành động được, không filler" không?

## 9. Release Checklist (L2 — khi tag `repo-vX.Y`)

- [ ] L1 xanh toàn repo, 0 BLOCK, mọi WARN có xác nhận
- [ ] CHK-14 link ngoài chạy, issue mở cho link chết
- [ ] Golden tests của mọi production prompt chạy pass trên model đương nhiệm (ghi model+ngày)
- [ ] CHANGELOG.md có mục cho tag; version các module đổi khớp diff thật
- [ ] WAIVERS: không waiver quá hạn; waiver còn sống có lý do còn đúng
- [ ] Biên bản release trong `records/releases/` — ai/agent nào ký, ngày, kết quả từng mục trên

## 10. KPI của chính hệ gate (gate cũng bị đo)

- **Escape rate:** lỗi lọt qua gate bị phát hiện muộn / tổng lỗi — mỗi escape sinh CHK mới (luật §2).
- **False-positive rate:** WARN bị người bác / tổng WARN — vượt ~30% hai tháng liên tiếp → nới heuristic (CHK-11 là ứng viên số 1).
- **Thời gian gate:** L0 >5s hoặc L1 >2 phút → tối ưu trước khi thêm check mới (gate chậm = gate chết, §0.4).
- **Waiver nợ:** số waiver sống + tuổi trung bình — tăng đều là governance đang mục từ bên trong.

## 11. Kế hoạch triển khai (bootstrap trên repo đang đỏ)

```
Bước 1  Viết tools/spec_lint.py với CHK-01..05,07,12 (lõi) + chạy trên repo hiện tại
        → kết quả ĐỎ là dự kiến: chính là B1–B4 của audit; output JSON = danh sách việc sửa
Bước 2  Sửa B1–B5 (đợt sửa đã duyệt ở audit) đến khi lõi xanh
Bước 3  Thêm CHK-06 generator + 08,09,10,11,13,16; wire CI + branch protection + PR template
Bước 4  Tag repo-v2.1 qua Release Checklist lần đầu — biên bản đầu tiên trong records/
Bước 5  Chỉ sau đó mới viết module 03 (STOP CONDITION của directive trước vẫn hiệu lực)
Ước lượng tổng: 1–2 ngày công, khớp ước lượng audit.
```

## 12. Self-Critique của blueprint

(1) **Nguy cơ lớn nhất không phải kỹ thuật mà là hành vi:** solo operator có toàn quyền tắt gate — phòng tuyến cuối là branch protection + văn hoá "waiver công khai có hạn" thay vì bypass lén; spec không thể ép, chỉ làm cho việc đúng rẻ hơn việc sai. (2) Heuristic CHK-11 (evidence) sẽ ồn — đã đặt WARN + ngân sách false-positive thay vì BLOCK, và đó là trade-off có chủ đích: thà ồn có kiểm soát hơn im lặng. (3) Toàn hệ đứng trên GitHub Actions cho L1 — đã giảm phụ thuộc bằng cách dồn logic vào CLI chạy offline; mất GitHub thì mất tự động hoá chứ không mất khả năng kiểm. (4) Chưa có gì kiểm *chất lượng nội dung domain* — cố ý: đó là việc của review người (§8) và evidence policy; gate máy giả vờ kiểm được nội dung sẽ tạo cảm giác an toàn giả. (5) 16 CHK cho repo 1 người là nhiều — nhưng 12/16 chạy tự động không tốn giờ người; chi phí thật nằm ở Bước 1–3 một lần, đúng nguyên tắc trả trước của audit F3.
```
