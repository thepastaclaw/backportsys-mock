MOCK ONLY — layout concept using historical Dash #7924 content; no backport or implementation changes, and no builds or tests newly run.

## Issue being fixed or feature implemented

The source PR, https://github.com/dashpay/dash/pull/7924, backports one Bitcoin Core v0.26 pull request: bitcoin#28364. This mock changes presentation only.

## What was done?

- Bitcoin Core #28364 — transaction/orphan logging and the TXPACKAGES category.
  Upstream: https://github.com/bitcoin/bitcoin/pull/28364 (merge 5666966dffeb620b4a628679d667346e02fd3b19). Dash adaptation: 7b8c1903d33aeac8ad33074b5dd6a4bfd7f58a88, recorded in Dash #7924.
  - Adds TXPACKAGES for orphanage/package-relay logging, moves orphan and orphanage messages into that category, and adds per-transaction erasure and peer-workset messages plus mempool acceptance/rejection messages for processed orphans. Moves the AlreadyHaveTx/recent-rejects DoS-score comment to the early-return path it describes.
  - In src/logging.h, uses bit 29 in Dash's uint64_t LogFlags enum, preserving Dash's category bits 32+ and ALL = ~(uint64_t)0. Registers txpackages in LogCategories and LogCategoryToStr alongside Dash's existing chainlocks, llmq and coinjoin categories.
  - In src/net_processing.cpp, omits wtxid from all new/changed messages because Dash has no SegWit or wtxids. ProcessOrphanTx uses node_id: Dash takes a NodeId rather than upstream's Peer&/peer.m_id. The TX-handler “not keeping orphan”, MEMPOOLREJ, ATMP-accepted and forcerelay messages stay unchanged because their only upstream change was adding wtxid.
  - In src/txorphanage.cpp, moves the orphanage logs to TXPACKAGES and adds EraseTxNoLock and workset logging without wtxid, retaining Dash's size-based orphan limit. The large-orphan message changes its label from “hash:” to “txid:” without adding a wtxid field. The commit records that test/functional/p2p_permissions.py expectations retain the no-“(wtxid=...)” format and Dash's mocktime in_mempool helper; that test file is not changed by this commit.
  - Intentionally omits src/validation.cpp's RBF “replacing tx” log change in MemPoolAccept::Finalize: Dash has neither RBF nor the m_all_conflicting loop. Keeps Dash's net_processing conflict context instead of upstream's witness-stripped m_recent_rejects logic and m_replaced_transactions loop, which have no Dash counterparts. These are intentional SegWit/RBF exclusions, not deferred prerequisites. No deferred prerequisite is named for this change in the source commit.

## How Has This Been Tested?

Historical source report from Dash #7924 for 7b8c1903d33aeac8ad33074b5dd6a4bfd7f58a88, not newly run or independently validated for this mock:

- Per-commit builds reusing the worktree cache; test suites touched by each diff, at that commit, where the diff selected one; a mechanical diff-of-diffs against upstream; and an independent verification pass on the one adapted commit. The source does not identify individual suites or claim an exhaustive test run.
- Fork gate PR https://github.com/thepastaclaw/dash/pull/102 at 7b8c1903d3: **no passing CI record at this head**. Detailed historical gate results and warnings remain in the initial mock comment.

## Breaking Changes

The historical source states: “None beyond the upstream changes themselves.” This mock does not revalidate that statement.

## Checklist:

Left for the reviewer; all boxes remain unchecked in this mock.

- [ ] I have performed a self-review of my own code
- [ ] I have made corresponding changes to the documentation
- [ ] I have assigned this pull request to a milestone _(for repository code-owners and collaborators only)_

Requested by knst via PastaClaw.
