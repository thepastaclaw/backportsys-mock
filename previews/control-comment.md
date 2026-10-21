## INERT MOCK — illustrative control comment

This comment is disconnected from backportsys. Checking these boxes does nothing: no update, rebase, close, acknowledgement or checkbox clearing is wired up. No production control markers or labels are present.

In the proposed real UI, a maintainer would tick a control for action within a few minutes, then the bot would clear it. For other requests (a hunk to drop, a resolution to redo, or a question), leave a review comment.

- [ ] 🔒 **Hands off** — illustrative: stop every automated update to this branch
- [ ] 🔄 **Rebase** — illustrative: rebase onto current develop
- [ ] ❌ **Close** — illustrative: abandon this batch and release its items

### Adaptation legend

Needs a careful look. The source orders commits by adaptation: 🔴 heavy first; 🟡 light (at most two invented lines, or up to six lines counting edits of upstream functional-test or doc lines); 🟢 faithful (identical to upstream apart from context). The source classifies this change as 🔴 heavy.

### Historical gates — copied from Dash #7924, NOT newly run

Source: https://github.com/dashpay/dash/pull/7924. The table below transcribes its live description at mock preparation time. All results refer to the original change, not this mock branch; they have not been rerun or independently validated in this job.

| Upstream change | Original recorded commit | Adaptation | build | ci_upstream | mech | pick | tests | tree | verify |
| --- | --- | --- | --- | --- | --- | --- | --- | --- | --- |
| bitcoin/bitcoin#28364 | 7b8c1903d3 | 🔴 heavy | pass | warn | warn | pass | pass | pass | pass |

Original warning details, preserved:
- bitcoin#28364 mech: **warn** — 10 invented line(s); 4 partial/prereq/low-risk finding(s).
- Fork gate PR https://github.com/thepastaclaw/dash/pull/102 at 7b8c1903d3 — **no passing CI record at this head**.

The mechanical check is described by the source as checking upstream-hunk presence, added-line counterparts and preservation of Dash-specific lines. The source body gives only the finding count above, not four individual finding descriptions or a separate explanation of ci_upstream:warn. No missing details are invented here, and the test pass entry does not claim that every test suite ran.

### Concept-review flow

Notification → skim title → open PR → scroll to this early control comment. This comment was posted one second after mock PR creation and remains the first comment; a real implementation need not beat other bots. A later real implementation must handle ordering races and existing PRs, whose first comment cannot be replaced retroactively. No implementation, deployment or production PR is changed by this mock; one consolidated implementation PR remains for after explicit approval.
