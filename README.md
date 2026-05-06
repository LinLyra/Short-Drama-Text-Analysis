# Short Drama Text Analysis: Human–AI Dialogue Behavior Mining

## Overview

This project focuses on processing and analyzing human–AI role-play dialogue data generated from internal AI training scenarios. The original dataset consisted of nested JSON conversation records containing employee information, practice metadata, evaluation scores, scenario descriptions, and multi-turn chat logs between users and AI assistants.

The goal of this project was to transform highly unstructured conversational data into a structured analytical dataset that could support:

- Dialogue behavior analysis
- AI training evaluation
- User engagement measurement
- Learning progression tracking
- Feature engineering for downstream modeling
- Dashboard and reporting systems

The project combines data processing, NLP feature engineering, interaction analysis, and semantic similarity modeling to better understand how employees interact with AI coaching systems over time.

---

# Project Objectives

The main objectives of this project were:

- Build a robust JSON-to-tabular processing pipeline
- Reconstruct complete human–AI conversations
- Generate interpretable session-level behavioral metrics
- Measure interaction quality and engagement
- Analyze practice rhythm and learning progression
- Quantify lexical and semantic novelty in conversations
- Export clean datasets for future analysis and modeling

---

# Dataset Structure

The original data was stored as nested JSON records.

Each practice session included:

- Employee information
- Department information
- Practice timestamps
- Evaluation scores
- Scenario metadata
- Multi-turn dialogue logs
- AI feedback and evaluation text

The raw structure was highly hierarchical and required extensive parsing before analysis.

---

# Processing Pipeline

## 1. JSON Parsing

The first step involved parsing nested JSON dialogue records and extracting:

- Practice ID
- Employee ID
- Employee name
- Practice time
- Scenario information
- Total score
- Evaluation text
- Conversation logs

The pipeline normalized inconsistent fields and removed invalid or incomplete records.

---

## 2. Message-Level Reconstruction

Each dialogue session was expanded into message-level rows.

For every message, the following information was preserved:

- Practice ID
- Speaker role (user / assistant)
- Message content
- Turn index
- Timestamp
- Session ordering

This reconstruction allowed downstream conversational analysis at both micro and macro levels.

---

## 3. Session-Level Aggregation

After message reconstruction, conversations were aggregated into practice-session-level datasets.

Each row represented one complete practice session.

Generated metrics included:

- Total dialogue turns
- User turns
- Assistant turns
- User-to-assistant balance
- Average message length
- Token statistics
- Question rate
- Interaction density
- Practice duration
- Session frequency

---

# Feature Engineering

## Behavioral Features

The project engineered interpretable behavioral indicators such as:

- Practice frequency
- Daily and weekly activity
- Gap since previous session
- Session order per employee
- Interaction balance
- Alternation rate
- Assistant dominance ratio

These features helped describe user learning rhythm and engagement patterns.

---

## Text Features

Text-level indicators included:

- Average token length
- Average character length
- Type-token ratio
- Lexical diversity
- Question frequency
- Repetition metrics

These metrics helped evaluate expression richness and interaction quality.

---

## Learning Progress Features

To track progression over time, the project generated:

- Numeric score extraction
- Score delta from previous session
- Rolling average scores
- Historical comparison indicators

These features provided visibility into short-term learning improvement trends.

---

# Novelty Analysis

One of the key focuses of this project was measuring novelty in employee responses.

## Lexical Novelty

Lexical novelty was measured using:

- TF-IDF cosine similarity
- Jaccard similarity

Each session was compared against the employee’s previous sessions.

Novelty was defined as:

```text
1 - maximum historical similarity
