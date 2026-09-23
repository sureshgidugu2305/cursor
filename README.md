# LEK Salesforce architecture

This repository holds architecture recommendations for the LEK (LEAK) Salesforce / Agentforce program.

## Documents

### Client deliverable

- **[LEAK Agentforce Enhancement Proposal (Word)](docs/LEAK-Agentforce-Enhancement-Proposal.docx)** — the formal proposal document for the client: executive summary, current state, gap analysis, proposed solution, advantages, roadmap, metrics, and risks.

### Supporting analysis

- [LEK Agentforce: Differences, Capabilities & Advantages](docs/LEK-Agentforce-Capabilities-Comparison.md) — side-by-side mid-2025 vs current Agentforce: what LEK has, what is available now, and business advantages.
- [LEK Agentforce Enhancement Roadmap](docs/LEK-Agentforce-Enhancement-Roadmap.md) — phased architecture and implementation roadmap from the mid-2025 baseline to Agentforce 360.

## Regenerating the Word proposal

The `.docx` is generated from a script so it can be version-controlled and updated consistently.

```bash
pip3 install python-docx
python3 tools/build_lek_proposal_docx.py
```

Edit `tools/build_lek_proposal_docx.py` and re-run to update the deliverable.
