## Architecture Decision Records (ADR)
This document captures key technical decisions made during the development of the RPA Challenge Automation Framework. Each decision includes context, options considered, and final verdict.

ADR-001: Locator Strategy for Dynamic Form Fields
Context : RPA Challenge presents a unique automation challenge: form fields change position after each submission. The same field (e.g., "First Name") appears in different locations on each iteration.

## The Problem:
Traditional locators (CSS, XPath by position) will break when fields move
Need a resilient strategy that survives DOM changes
Must maintain test stability across all 10 form iterations

## Options Considered:
#Option A: CSS Selectors by Position
page.locator("input:nth-child(1)").fill("John")

Pros: Faster execution and simple syntax
Cons: 
Breaks immediately when field position change
Requires upadting selector5s after each iteration
High maintenance, low reliability

Verditc :Rejected- Not sutiable for dynamic usecase

Option B : 
