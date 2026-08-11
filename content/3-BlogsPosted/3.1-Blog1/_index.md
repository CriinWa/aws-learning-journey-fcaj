---
title: "Blog 1"
date: 2026-08-02
weight: 1
chapter: false
pre: " <b> 3.1. </b> "
---

# KIRO POWERS

When vibe coding, FCAJ members often face these issues:

- Every new project needs to be configured again from the beginning.
- Teams find it difficult to keep the same setup, which can lead to inconsistent code.
- Loading every tool at once causes context bloat, wastes tokens, and reduces the AI's focus.

Kiro Powers was created to solve this problem. Instead of manually configuring every new project, we can package tools, instructions, and automation into one unit and share it with the whole team. After one installation, everyone uses the same setup.

A Power is a collection of components:

- A `POWER.md` file containing documentation and activation keywords.
- MCP servers that provide execution tools.
- Steering files that define workflows and standards.
- Hooks that automate actions based on events.

The important difference is that a Power is **not preloaded**. It activates only when a prompt contains the right keyword. This gives an agent only the knowledge and tools it needs, avoiding context bloat.

Anyone can create a Power with Build a Power, or import one from GitHub or a local folder through Add Custom Power. After it is complete, the team can push it to a Git repository and install the same setup together.

## Quick example: Zapier Power

After installing the “Zapier” Power in the Kiro Powers panel, an agent can connect to and automate thousands of external applications.

This Power includes `POWER.md` keywords such as `zapier`, `automation`, `webhook`, `youtube`, and `discord`, together with the Zapier MCP and steering files for data-integration workflows. A prompt such as “Get the latest video from YouTube channel Y and send it to channel X...” or a Zapier workflow link can activate the Power. The agent then gets connection context, maps data, creates the message format, and automates the flow instead of requiring manual code.

Kiro Powers is more than a utility. It is a way to turn expertise into reusable modules that can be shared with a team.

{{< event-image src="images/3-Blog/Blog1.jpg" alt="Blog 1" >}}

## References

1. [Kiro Powers documentation](https://kiro.dev/docs/powers/)
2. [Introducing Kiro Powers](https://kiro.dev/blog/introducing-powers/)
3. [Kiro Powers introduction video](https://youtu.be/kEOmuVyqfMU?si=p9iFGMNMUK9rbYAp)

## Link Post

[AWS Study Group post](https://www.facebook.com/groups/awsstudygroupfcj/permalink/2232309990867294/)
