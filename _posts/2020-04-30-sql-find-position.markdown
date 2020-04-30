---
layout: post
title:  "SQL - Find Position of Record in Results"
date:   2020-04-30 00:00:00 -0500
categories: sql
---
Similar to my [previous post]({% post_url 2020-05-28-activerecord-compound-column-uniqueness %}), I have a `high_scores` table that tracks users' high scores for several games.
I needed to return the current user's global rank for a specified game.

Here's the table structure:

```sql
CREATE TABLE `high_scores` (
  `id` bigint(20) NOT NULL AUTO_INCREMENT,
  `game_id` bigint(20) NOT NULL,
  `user_id` bigint(20) NOT NULL,
  `score` int(11) DEFAULT '0',
  PRIMARY KEY (`id`),
  KEY `index_high_scores_on_game_id` (`game_id`),
  KEY `index_high_scores_on_user_id` (`user_id`),
  KEY `index_high_scores_on_game_id_and_user_id` (`game_id`,`user_id`),
  KEY `index_high_scores_on_game_id_and_score` (`game_id`,`score`)
);
```

And here's the query I use to get the user's global rank for a single game:

```sql
SELECT h1.user_id, h1.score, (
  SELECT COUNT(*) FROM high_scores h2 WHERE h2.score > h1.score AND h2.game_id = 3
) + 1 AS rank
FROM high_scores h1
WHERE h1.game_id = 3 AND h1.user_id = 57;
```

Note that I needed to add 1 to the count because it only return how many other scores are higher, and we want the user's rank.
