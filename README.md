# QuorumVote app

A web app for [QuorumVote](https://github.com/Obitotobi1/quorumvote), a GenLayer Intelligent Contract where validators grade every free-text answer against the poll's rubric. Only answers that pass are counted.

**Live app:** https://obitotobi1.github.io/quorumvote-app/

**Contract (GenLayer Studio):** `0x88E5eD7BcA3f474449AeB723711c99fc03347A0d`

Explorer: https://explorer-studio.genlayer.com/address/0x88E5eD7BcA3f474449AeB723711c99fc03347A0d

## What you can do

1. **Create a poll** with a question and a rubric (what a passing answer must do).
2. **Answer a poll** in your own words (one answer per account, 8 to 400 characters).
3. **Ask validators to grade an answer.** Each validator runs the model against the rubric. A verdict is kept only if the quote it relies on appears word for word in the answer. Otherwise the answer stays pending and can be graded again (3 tries).
4. **Read the tally.** It counts passing answers only, with a bar for passed, failed and not graded yet.
5. **Close a poll** (owner only).

## How the app handles results

A transaction is only shown as a success when the network reports ACCEPTED or FINALIZED **and** the app reads the record back and sees the change:

| What happened | What you see |
|---|---|
| Accepted and the poll, answer or verdict really changed | Accepted by validator consensus, then Finalized |
| Receipt without any status | "Not confirmed: the network reported no status" |
| Accepted but the record is identical | "Ran, but no change was stored" |
| Validators disagree, timeout, cancel | The real name of the failure, never a success |
| Page reload or network error mid-way | "Nothing is confirmed" and a pointer to the Explorer |

Text from users is always escaped, so a hostile question or answer is shown as text and never runs.

## Demo accounts

The app uses two throwaway demo accounts generated in your browser (no wallet, no funds). Switch between Account A and B to answer the same poll as two different people. This is a Studio test-network demo.

## Tests

Run in a headless browser against a mock of the network:

- Create, answer, grade and close all work end to end.
- Receipt with no status is never a success.
- Accepted but nothing stored is reported as such.
- Hostile HTML in a question does not execute.

The contract has its own 13 local tests in the [QuorumVote contract repo](https://github.com/Obitotobi1/quorumvote).

## Run it

It is a single static file: open `index.html`, or host it anywhere (GitHub Pages works). The contract address can be changed in the app.
