# Project Status

**Last Updated:** 2026-01-08

## Current State

BMR Framework is a specification repository defining an AI-driven research orchestration system for magnet and electromagnetic device R&D. The core framework documents are in place: tool registry v0.2.0 (JSON/YAML), Scientific Action Language (SAL.md), agent prompts for 7 roles (agent_prompts.md), and magnet taxonomy (magnet_taxonomy_v1.md). Five stub documents exist as placeholders for future expansion. Repository is now version-controlled and published to GitHub.

## Recent Changes
- Created `CLAUDE.md` - guidance for future Claude instances
- Created `.gitignore` - excludes .DS_Store, .claude/, IDE files
- Initialized git repository with `main` branch
- Published to GitHub: https://github.com/Jabberwockyjib/BMR-Framework

## Known Issues
- Five stub documents (BMR-*.md) contain only placeholder titles, need content
- No README.md for GitHub landing page
- No JSON schema validation for tool registry structure

## Architecture Notes

This is a **specification repository**, not executable code. Key documents:

| File | Lines | Purpose |
|------|-------|---------|
| `tool_registry_moat_spec_v0_2_0.json` | 753 | Primary spec: physics graph, KPIs, implementations |
| `agent_prompts.md` | 304 | System prompts + JSON schemas for 7 agent roles |
| `SAL.md` | 171 | Scientific Action Language methodology |
| `magnet_taxonomy_v1.md` | 175 | Problem taxonomy (9 categories) |

The framework is designed for LLM agents to consume and orchestrate physics simulations via tools like Elmer, FEMM, MuMax3, OpenFOAM, and AiiDA.
