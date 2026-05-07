# Product Leadership Skills

A collection of Claude skills for product leaders — automate OKRs, roadmaps, competitive analysis, and stakeholder communication.

## Skills Included

### `okr-cascade`
Convert a hierarchical Notion OKR template into a pre-populated Excel tracker, automatically cascading company objectives → portfolio KRs → team OKRs.

**Use when:** You have a quarterly strategy in Notion and need to generate team OKRs fast.

**Time saved:** ~3-4 hours per quarter.

```
/okr-cascade Q2 2026 [paste notion template here]
```

Output: `OKR_Tracker_[Quarter].xlsx` with all company, portfolio, and team objectives pre-filled.

## Installation

1. Clone this repo (or download the skill folder)
2. In Claude Code, go to Settings → Skills → Add Custom Skill
3. Point to the skill folder: `skills/okr-cascade/`
4. The skill is now available as `/okr-cascade`

## Examples

See `skills/okr-cascade/examples/` for a sample Notion template input and the generated Excel output.

## Contributing

Have an improvement? Fork this repo and submit a PR. All contributions welcome.

## License

MIT
