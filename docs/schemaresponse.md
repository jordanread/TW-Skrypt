Excellent. Treating TW-Skrypt like a **schema-driven system** will make the rest of the plugin (widgets, templates, automation) much easier to maintain. The goal here is to define a **canonical field schema registry** that the plugin can reference when creating or validating tiddlers inside **TiddlyWiki.

This step does **NOT** create the templates themselves. Instead it defines the **field contracts** that templates must follow.

---

# TW-Skrypt Canonical Field Schema System

## Purpose

The schema system will:

• document fields for every entity type
• drive tiddler creation widgets
• power form generation later
• allow validation of required fields
• allow migration if schema evolves

---

# Schema Storage Location

All schemas should live under:

```
$:/skrypt/schema/
```

Each entity type gets its own schema definition.

Example:

```
$:/skrypt/schema/story
$:/skrypt/schema/manuscript
$:/skrypt/schema/chapter
$:/skrypt/schema/scene
$:/skrypt/schema/character
$:/skrypt/schema/location
$:/skrypt/schema/item
$:/skrypt/schema/organization
$:/skrypt/schema/timeline-event
$:/skrypt/schema/research
$:/skrypt/schema/note
$:/skrypt/schema/outline
$:/skrypt/schema/beat
```

---

# Schema Format

Each schema tiddler should be a **data tiddler (JSON)**.

Example structure:

```
{
  "entity": "scene",
  "description": "Core writing unit",
  "required": [
    "story-id",
    "skrypt-type",
    "skrypt-id"
  ],
  "fields": {
    "story-id": {
      "type": "string",
      "description": "Slug identifying the story"
    },
    "scene-order": {
      "type": "number",
      "description": "Scene order inside chapter"
    }
  }
}
```

Later the plugin can read this using:

```
<$data tiddler="$:/skrypt/schema/scene"/>
```

---

# Universal Fields (Every Entity)

These fields **must exist on every TW-Skrypt content tiddler**.

| Field       | Type   | Purpose           |
| ----------- | ------ | ----------------- |
| story-id    | string | story slug        |
| skrypt-type | string | entity type       |
| skrypt-id   | string | unique slug       |
| created     | date   | creation time     |
| modified    | date   | last modification |

Example:

```
story-id: iron-empire
skrypt-type: scene
skrypt-id: arrival-at-harbor
created: 202603101200
modified: 202603101210
```

---

# Story Schema

Schema location

```
$:/skrypt/schema/story
```

Fields

| Field            | Type   | Required | Description          |
| ---------------- | ------ | -------- | -------------------- |
| story-id         | string | yes      | canonical story slug |
| title            | string | yes      | story title          |
| author           | string | no       | author               |
| genre            | string | no       | lookup reference     |
| subgenre         | string | no       | secondary genre      |
| logline          | string | no       | short pitch          |
| premise          | text   | no       | longer concept       |
| status           | string | no       | planning/draft/etc   |
| target-wordcount | number | no       | goal wordcount       |
| structure-model  | string | no       | story structure      |
| created          | date   | yes      | timestamp            |
| modified         | date   | yes      | timestamp            |

---

# Manuscript Schema

Location

```
$:/skrypt/schema/manuscript
```

Fields

| Field            | Type   | Required |
| ---------------- | ------ | -------- |
| story-id         | string | yes      |
| skrypt-type      | string | yes      |
| skrypt-id        | string | yes      |
| title            | string | yes      |
| draft-number     | number | no       |
| status           | string | no       |
| wordcount-target | number | no       |
| created          | date   | yes      |
| modified         | date   | yes      |

---

# Chapter Schema

Location

```
$:/skrypt/schema/chapter
```

Fields

| Field         | Type   | Required |
| ------------- | ------ | -------- |
| story-id      | string | yes      |
| manuscript-id | string | yes      |
| skrypt-type   | string | yes      |
| skrypt-id     | string | yes      |
| title         | string | yes      |
| chapter-order | number | yes      |
| summary       | text   | no       |
| status        | string | no       |
| created       | date   | yes      |
| modified      | date   | yes      |

---

# Scene Schema

This is the **most important schema**.

Location

```
$:/skrypt/schema/scene
```

Fields

| Field              | Type   | Required |
| ------------------ | ------ | -------- |
| story-id           | string | yes      |
| manuscript-id      | string | yes      |
| chapter-id         | string | yes      |
| skrypt-type        | string | yes      |
| skrypt-id          | string | yes      |
| title              | string | yes      |
| scene-order        | number | yes      |
| pov-character      | string | no       |
| location           | string | no       |
| time               | string | no       |
| status             | string | no       |
| summary            | text   | no       |
| goal               | text   | no       |
| conflict           | text   | no       |
| outcome            | text   | no       |
| characters-present | list   | no       |
| wordcount          | number | no       |
| created            | date   | yes      |
| modified           | date   | yes      |

---

# Character Schema

Location

```
$:/skrypt/schema/character
```

Fields

| Field            | Type   | Required |
| ---------------- | ------ | -------- |
| story-id         | string | yes      |
| skrypt-type      | string | yes      |
| skrypt-id        | string | yes      |
| name             | string | yes      |
| role             | string | no       |
| archetype        | string | no       |
| age              | number | no       |
| gender           | string | no       |
| description      | text   | no       |
| goals            | text   | no       |
| motivation       | text   | no       |
| conflict         | text   | no       |
| character-arc    | text   | no       |
| first-appearance | string | no       |
| created          | date   | yes      |
| modified         | date   | yes      |

---

# Location Schema

Location

```
$:/skrypt/schema/location
```

Fields

| Field            | Type   | Required |
| ---------------- | ------ | -------- |
| story-id         | string | yes      |
| skrypt-type      | string | yes      |
| skrypt-id        | string | yes      |
| name             | string | yes      |
| location-type    | string | no       |
| region           | string | no       |
| description      | text   | no       |
| culture          | text   | no       |
| first-appearance | string | no       |
| created          | date   | yes      |
| modified         | date   | yes      |

---

# Item Schema

Location

```
$:/skrypt/schema/item
```

Fields

| Field            | Type   | Required |
| ---------------- | ------ | -------- |
| story-id         | string | yes      |
| skrypt-type      | string | yes      |
| skrypt-id        | string | yes      |
| name             | string | yes      |
| type             | string | no       |
| owner            | string | no       |
| description      | text   | no       |
| first-appearance | string | no       |
| created          | date   | yes      |
| modified         | date   | yes      |

---

# Organization Schema

Location

```
$:/skrypt/schema/organization
```

Fields

| Field       | Type   |
| ----------- | ------ |
| story-id    | string |
| skrypt-type | string |
| skrypt-id   | string |
| name        | string |
| type        | string |
| leader      | string |
| members     | list   |
| description | text   |
| created     | date   |
| modified    | date   |

---

# Timeline Event Schema

Location

```
$:/skrypt/schema/timeline-event
```

Fields

| Field        | Type   |
| ------------ | ------ |
| story-id     | string |
| skrypt-type  | string |
| skrypt-id    | string |
| title        | string |
| year         | number |
| month        | number |
| day          | number |
| era          | string |
| location     | string |
| participants | list   |
| summary      | text   |
| importance   | string |
| created      | date   |
| modified     | date   |

---

# Research Schema

Location

```
$:/skrypt/schema/research
```

Fields

| Field       | Type   |
| ----------- | ------ |
| story-id    | string |
| skrypt-type | string |
| skrypt-id   | string |
| title       | string |
| source      | string |
| author      | string |
| url         | string |
| notes       | text   |
| tags        | list   |
| created     | date   |
| modified    | date   |

---

# Note Schema

Location

```
$:/skrypt/schema/note
```

Fields

| Field       | Type   |
| ----------- | ------ |
| story-id    | string |
| skrypt-type | string |
| skrypt-id   | string |
| title       | string |
| tags        | list   |
| created     | date   |
| modified    | date   |

---

# Outline Schema

Location

```
$:/skrypt/schema/outline
```

Fields

| Field           | Type   |
| --------------- | ------ |
| story-id        | string |
| skrypt-type     | string |
| skrypt-id       | string |
| title           | string |
| structure-model | string |
| created         | date   |
| modified        | date   |

---

# Beat Schema

Location

```
$:/skrypt/schema/beat
```

Fields

| Field         | Type   |
| ------------- | ------ |
| story-id      | string |
| skrypt-type   | string |
| skrypt-id     | string |
| beat-order    | number |
| beat-type     | string |
| description   | text   |
| related-scene | string |
| created       | date   |
| modified      | date   |

---

# Instructions for Inserting Schemas

These should be **shadow tiddlers in the plugin**.

Example plugin tiddler:

```
title: $:/skrypt/schema/scene
type: application/json
```

Content:

```
{schema JSON}
```

---

# How the Plugin Will Use These

Later systems will read the schema to:

### Create new entities

```
<<skrypt-new scene>>
```

Steps:

1. load schema
2. create fields
3. populate defaults
4. open editor

---

### Validate tiddlers

Example macro:

```
<<skrypt-validate scene>>
```

Checks required fields.

---

### Generate forms automatically

Schema can drive UI widgets like:

```
field editors
dropdowns
checkbox lists
```

---

# Critical Next Component

Now that the **data model exists**, the next piece to design is the **TW-Skrypt Creation Engine**.

This is the widget/macro system that will:

• create tiddlers from schemas
• auto-generate IDs
• apply template sets
• enforce story-id relationships

Without this engine the plugin becomes extremely tedious to use.

---

If you'd like, I can also design the **TW-Skrypt Creation Engine architecture**, which is the part that makes the plugin feel like **Scrivener inside TiddlyWiki rather than just structured notes**.
