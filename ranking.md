# Ranking

How Orbit decides what you see first. There are no paid placements, promoted posts or hidden boosts: every signal used is listed here.

**Sparks** are Orbit's upvotes. Views and reports are **not** ranking signals.

## Hot

Hot balances engagement against age:

```
engagement = max(1, sparks + 2 × comments)
hot        = log10(engagement) + seconds_since_epoch / 45000
```

- A comment counts twice as much as a spark, because a conversation takes more effort than a tap.
- Engagement is on a log scale: the first 10 sparks matter as much as the next 90.
- Every **12.5 hours** of age costs as much as a 10× drop in engagement, so the front page keeps moving.

Hot is the default inside an orbit.

## New

Newest first. Nothing else.

## Top

```
top = sparks + min(comments, 999) / 1000
```

Sparks decide; comments only break ties. You choose the time range (default: past week).

## Rising

Rising exists to surface new voices, not the usual top posters:

- Posts from the last **72 hours**.
- Only from members whose [aura](aura.md) is **at or below the median** of the authors in the pool.
- Ranked by velocity, so fast-growing posts beat old big ones:

```
velocity = (sparks + 2 × comments) / (age_hours + 2)^1.2
```

## For You

The default home feed. It starts from Hot posts in the orbits you joined plus popular posts across Orbit, then adjusts for you:

| Signal | Effect |
| --- | --- |
| Posts from people you follow | +0.3 (about 12.5 hours of freshness) |
| Posts people you follow are active in | +0.15 |
| New posts in your orbits (first 6 hours) | Age counts half as much |
| Fresh posts in your orbits (last 24 hours) | Mixed in so they aren't missed |
| Posts you've already opened | Moved down, unless 3+ new comments arrived since |
| Your own posts from the last hour | Shown first, only to you |
| Your languages | Posts in languages you read are preferred |
| Variety | One orbit or author can't fill the whole feed |

## Languages

If you choose specific languages, Hot, New and Top show posts in those languages. When English is a secondary language for you, English posts get a smaller weight so your primary language comes first. Search uses the same preference.

## Today

A short daily summary: up to five of the best posts from your orbits since your last visit (between 1 and 72 hours; 24 by default), plus a few of the day's top posts across Orbit. Posts are chosen by `sparks + 2 × comments`. Labels like "Debated" or "Active thread" explain why a post is there; they don't change the order.

## Comments

| Sort | Order |
| --- | --- |
| **Best** (default) | `likes + 6 × e^(−age / 4 hours)` |
| Top | Most likes, then oldest first |
| New | Newest first |
| Old | Oldest first |

Best gives every new comment a short head start (worth about 6 likes, fading over a few hours), so fresh voices are seen before a thread settles. The same sort applies at every reply level.

## Explore

- **Trending orbits:** ranked by growth this week (new replies or new members).
- **For you:** orbits in categories you already like, weighted by size and today's activity.
- **Rising orbits:** orbits younger than 120 days, scored by `(members + 5 × active_today) / √age_days`, so small active orbits can be found.
