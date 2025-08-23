# Understanding Fork Differences: A Guide for Interacting with AI

## Introduction
Comparing Git repositories, especially a fork against its upstream, can be tricky for AI models. This document explains why and, more importantly, how you can ask your AI to get the precise information you need without needing to understand complex Git internals or Boolean logic.

## The Core Problem: AI's View of Git

AI models sometimes struggle with Git comparisons because they tend to:
*   **See Git as a "Story," not a "Map":** We often process changes as a chronological sequence of events, which can obscure the underlying structural differences.
*   **Treat Dynamic References as Static:** Concepts like `HEAD`, branch names (`main`, `origin/main`, `upstream/main`) are constantly moving pointers in Git. AI can mistakenly treat them as fixed, unchanging locations.
*   **Get Confused by "Moving Targets":** When comparing two evolving branches, the reference points themselves are changing, making it hard for AI to establish a stable baseline for comparison.

This can lead to "flailing" or seemingly contradictory answers, as the AI tries to reconcile its narrative understanding with the precise, relational nature of Git.

## What You Really Want: The "What the Fork is About" Question

When you ask about differences between a fork and its upstream, you typically want to know:
*   What features or code are **unique to your fork**?
*   What changes have been made **only here** and not in the original repository?
*   What is the **net difference** that defines *this* version?

This is often expressed in set theory as: `Δ = Fork ∧ ¬Upstream` (changes in the Fork AND NOT in the Upstream).

## How to Ask Your AI: Simple, Effective Phrases

To help your AI provide accurate and concise answers about fork differences, use these phrases:

*   "Show me what's **unique** in *this* repository compared to the original."
*   "What changes have been made **only** in this fork?"
*   "Show me the features or code that are **unique to this version**."
*   "Compare this repository with its upstream, focusing on **what's new here**."
*   "What are the **differences that originated in this fork**?"

## Why This Helps Your AI

Using these phrases helps your AI by:
*   **Forcing Set-Based Reasoning:** It signals that you're looking for a precise "set difference" between the two repositories, rather than a chronological narrative of all changes.
*   **Clarifying the Reference Frame:** It helps the AI correctly identify the point of divergence (the common ancestor) and focus only on the changes that occurred *after* that point in your fork.
*   **Reducing Ambiguity:** It prevents the AI from getting lost in the "story" of upstream changes or reverted commits, and instead directs it to the core question of what makes your fork distinct.

By using these clear and direct questions, you can guide your AI to provide more accurate, less confusing, and ultimately more helpful responses when discussing Git fork differences.