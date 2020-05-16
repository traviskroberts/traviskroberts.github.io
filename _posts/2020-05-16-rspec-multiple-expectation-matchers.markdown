---
layout: post
title:  "RSpec Expect Multiple Possible Values"
date:   2020-05-16 00:00:00 -0500
categories: rspec rails
---
I was writing a unit test recently to test the return of a `RAND()` mySQL call and wanted to be able to expect one value or the other. I found out that RSpec has this ability built-in, but I'd never heard of it before.

From the RSpec documentation:

```ruby
class StopLight
  def color
    %w[green yellow red].shuffle.first
  end
end

RSpec.describe StopLight, "#color" do
  let(:light) { StopLight.new }

  it "is green, yellow or red" do
    expect(light.color).to eq("green").or eq("yellow").or eq("red")
  end

  it "passes when using boolean OR | alias" do
    expect(light.color).to eq("green") | eq("yellow") | eq("red")
  end
end
```
