---
layout: post
title:  "ActiveRecord Uniqueness Validation on Compound Columns"
date:   2020-04-28 00:00:00 -0500
categories: activerecord rails
---
I recently needed to add a uniqueness validation for compound columns in a Rails project.

I have a `high_scores` table that tracks users' high scores for several games. I wanted to ensure that each user only had one record per game.

```ruby
class HighScore < ApplicationRecord
  belongs_to :game
  belongs_to :user

  validates :game_id, presence: true
  validates :user_id, presence: true, uniqueness: { scope: :game_id }
end
```

Adding the scope to the uniqueness validation ensures that only one combination of user_id/game_id is allowed in the table.

### Bonus: Add a db-level constraint as well

```ruby
class AddGameUserIndexToHighScores < ActiveRecord::Migration
  def change
    add_index :high_scores, [:game_id, :user_id], unique: true
  end
end
```
