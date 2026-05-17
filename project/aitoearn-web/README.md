# Aitoearn Website

## Address

https://aitoearn.ai

# AiToEarn — The Best Open-Source AI Content Growth & Monetization Platform

## The core of AiToEarn is the Agent (Content Agent)

Each Agent represents a configurable, continuously working AI creator responsible for the full flow from content generation to publishing.

An Agent does more than generate content. It completes the full loop: **Create → Adapt → Publish**.

## Account binding (bind once, use continuously)

- Supports binding one Agent to multiple social media accounts
- Bound accounts can be reused long-term without repeated setup
- One Agent can manage multiple platform accounts at the same time

## Supported platforms

TikTok / YouTube / Instagram

X (Twitter) / Facebook / LinkedIn

Rednote / Douyin / Bilibili / Kuaishou, etc.

# Documents and resources

Official docs (Help Center): https://docs.aitoearn.ai/

Official website: https://aitoearn.ai/

# Frontend architecture and tech stack

This project is the frontend for the AiToEarn official site / web app, built on Next.js 14 (App Router), with a modular design around Agent-driven content creation and publishing workflows.

The primary goal is to support complex interaction and state management for continuous Agent creation, account binding, and multi-platform publishing.

# Development environment

Node.js & pnpm

pnpm run dev

Automated test commands:

npx playwright test tests/e2e/home/simple-test.spec.ts --headed --project=chromium

pnpm run test:agent

pnpm run test:agent:quick
