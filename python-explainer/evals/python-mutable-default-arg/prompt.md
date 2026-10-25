---
tags: [skill-creator]
max_turns: 20
timeout_seconds: 600
allowed_tools: [Read, Glob, Grep, Skill, Write, Edit, Bash]
---

why does this print [1, 2] the second time?

def add(x, lst=[]):
    lst.append(x)
    return lst

print(add(1))
print(add(2))
