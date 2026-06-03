---
name: using-gpus
description: Juliana's GPU compute across GCP/AWS/Azure — what exists, quotas, costs, launch commands, and hard rules. Use whenever launching, choosing, or budgeting GPU compute for training/inference/eval runs, or when the user asks where to run a job.
---

# Using GPUs

Info current as of 2026-08-14. Verify quotas with the commands below if it matters; update this file when things change.

## Where to run what

| Workload | Run it on |
|---|---|
| Large single-node fine-tune (needs 80GB VRAM) | GCP A100-80GB ×4, us-central1 |
| Multi-GPU / 8-GPU training | GCP H100 ×8 via DWS Flex Start (see below) |
| Many parallel small training jobs | GCP L4 ×8 (any US region, spot) or AWS L40S |
| Light inference: scoring, rendering, captioning | Cheapest available: L4 / T4 / A10G |
| Anything on Azure beyond a T4 | Don't — see Azure rules |

Project-specific workload→GPU mappings live in each project's own docs (e.g. DiT-LoRAcle's `PLAN.md`).

## GCP — primary (~$25k credits)

Project `project-ca10b891-b467-44b6-b56`, account `25julianal@gmail.com`.

- **Granted quota:** preemptible H100 ×64 + H100-Mega ×64 in ALL regions (project default; us-west1 has a stale ×8 override); A100-80GB ×4 (on-demand + preemptible, us-central1); preemptible A100-40GB ×64 + L4 ×16 (us-central1; other US regions ≥16/8); L4 ×8 + T4 ×4 on-demand.
- **H100s = DWS Flex Start** (consumes preemptible quota but VMs are held for the requested duration, no early preemption; jobs up to 7 days). Guide: https://docs.cloud.google.com/compute/docs/gpus/gpudirect#flex-start
- **Run Flex Start in the capacity regions:** us-central1 or europe-west (A3 High), asia-southeast1-c (A3 Mega). us-west1's incoming Mega capacity is CUD-only — don't wait for it.
- **Skip CUDs** (Sept 14, $8/$5 per GPU-hr, 1/3-yr commit) — project is ~2 months.
- Check quota: `gcloud compute regions describe us-central1 --project=project-ca10b891-b467-44b6-b56 --format=json | jq '.quotas[] | select(.metric|test("A100|H100|L4"))'`
- Escalation: rep Andre Garcia (andregarcia@google.com) + engineer Mayowa (mawojuyigbe@google.com) — fast, use them. Gemini API limits: Naji (najibarnes@google.com), project number 64890506934.

## AWS — secondary (~$25k credits, us-east-1, account 830109457500)

- Instances: `cs2881r-workhorse` (g6e.8xlarge, L40S 48GB — **course-named, never stop without asking Juliana**), `cs2881r-hardening` (g5.8xlarge, A10G), `nla-exp4-qwen` (g6e.8xlarge), all reusable.
- Quota: 64 on-demand G/VT vCPUs (= 2× g6e.8xlarge); 128 requested (spot case pending use-case reply in Support Center; on-demand rejected → AWS Sales only).
- **P-family (H100/A100): quota 64 vCPU but zero usage history.** Only fitting type is p5.4xlarge (1× H100, ~$6.9/hr) — us-east-1 capacity was dry 08-12. Rule: run something real on P first, then request more.
- Launch clone: AMI `ami-012ba162b9cd2729c` (DLAMI PyTorch 2.7), key `cs2881r`, SG `sg-007ca52c2b6cedb1f`.
- Quota-case replies go in the **Support Center console**, never by email (bounces).

## Azure — captioning only (~$100k credits, mostly unusable)

- **Do NOT file GPU quota via API or portal self-serve** — sponsorship subscription (`7205ad8e-7974-4232-bfe9-9dc1d480bc57`); capacity team denied GPU twice; API returns ContactSupport for every family. Only channel: Ajay Babu (v-ajbab@microsoft.com, MfS advisor).
- Usable: T4 48 vCPU swedencentral. VM `ditloracle-t4-caption` (`az vm start -g rg-ditloracle-swedencentral -n ditloracle-t4-caption`, ssh juliana@20.240.250.7, deallocate after use).
- Login: `az login --tenant 4da475c8-ebdd-4f7a-9627-0a8300526ad1` (MFA).

## Hard rules

1. Prefer GCP (credits + best quota). Never leave instances running idle — deallocate/stop after jobs (the workhorse accrued ~$650 idling in August).
2. All training jobs must checkpoint + auto-resume (everything preemptible-class assumes it).
3. Quota requests: cite the YC + Harvard research justification; never re-file something already denied on the same channel (check `memory/cloud-compute-inventory.md` history first).
