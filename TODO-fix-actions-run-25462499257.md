# TODO: Fix GitHub Actions failures in run 25462499257

Reference run: https://github.com/ThoSchGer/realm-dotnet/actions/runs/25462499257  
Workflow: `PR Build`  
Ref: `190b062daa01ff7b03c5ff56e0627b57c7ab16fc`

## Summary

The failing job log shows:

- `Resource not accessible by integration`

This indicates a GitHub Actions token/permissions problem rather than a build or test compilation failure.

## Checklist

- [ ] Identify the exact failing step in run `25462499257`, job `74707943884`.
- [ ] Confirm whether the error comes from one of these likely candidates:
  - `dorny/test-reporter`
  - `coverallsapp/github-action`
  - reusable workflow `./.github/workflows/wrappers.yml`
- [ ] Add an explicit top-level `permissions:` block to `.github/workflows/pr.yml`.
- [ ] Start with least privilege and verify whether these are sufficient:
  - `contents: read`
  - `checks: write`
  - `pull-requests: write`
- [ ] Inspect `./.github/workflows/wrappers.yml` for actions that also need explicit permissions.
- [ ] Verify whether the failing run was triggered from a forked pull request.
- [ ] If this is a forked PR, gate steps that require write access or secrets so they skip safely.
- [ ] Harden report publishing steps to skip gracefully when permissions are unavailable.
- [ ] Review all `dorny/test-reporter` steps in `.github/workflows/pr.yml`.
- [ ] Review the `coverallsapp/github-action` step in `.github/workflows/pr.yml`.
- [ ] Re-run the workflow after changes.
- [ ] Confirm there is no longer any `Resource not accessible by integration` error.
- [ ] Confirm downstream package/test/report jobs complete successfully.

## Suspicious workflow locations

- Test reporter steps:
  - `.github/workflows/pr.yml` lines 312-321
  - `.github/workflows/pr.yml` lines 391-400
  - `.github/workflows/pr.yml` lines 438-447
  - `.github/workflows/pr.yml` lines 491-500
  - `.github/workflows/pr.yml` lines 555-564
  - `.github/workflows/pr.yml` lines 595-604
  - `.github/workflows/pr.yml` lines 628-636
  - `.github/workflows/pr.yml` lines 671-679
  - `.github/workflows/pr.yml` lines 737-746
- Coveralls publish:
  - `.github/workflows/pr.yml` lines 727-735

## Notes

- `Resource not accessible by integration` usually means missing `GITHUB_TOKEN` permissions or fork PR restrictions.
- Prefer least-privilege permissions and explicit conditions for forked PRs.
