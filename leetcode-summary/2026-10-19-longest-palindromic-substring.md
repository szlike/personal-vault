# Longest Palindromic Substring

**Recorded:** 2026-10-19

## Pattern

Expand around center. Every position in the string is a possible center, and every gap between two positions is also a possible center, which gives 2n-1 centers in total, where n is the length of the string. Centers on a character cover odd-length palindromes, and centers in a gap cover even-length palindromes. Expand outward from each center while the characters on both sides match, and keep the longest match found.

## Context

This was pasted on the morning of the Pinterest tech screen on 2026-10-19 as a warm-up, and a full walkthrough was delivered the same day. No bug was found in the transcript.

## Past mistakes & points to improve

- None outstanding. This was banked as a screen-day warm-up with no bug found.

## Source

Prepared by Copper bot from Robin's interview practice sessions, September–November 2026. Ask Copper bot for more detail on anything in this note.
