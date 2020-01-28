---
layout: post
title: First Post of my blog
subtitle: words smell fresh here!
comments: false
published: true
---
**2020** is here! And my birthday too!

After attending [Codemash](https://www.codemash.org/) 2020 at Sandusky, I am greatly inspired by the technology advancement happening in the industry! I am also surprised to see many attendees passionate about what they work on!

I picked up few good learnings from the sessions, especially regarding GIT, Web authentication/authorization. I will try to post my learnings here as separate individual blog posts.

Hopefully, this blog gives me enough motivation to learn more and post!

Happy reading!

```c#
public virtual string Version {
            get {
                if(this._version == null) {
                    this._version = FileVersionInfo.GetVersionInfo(this.GetType().GetTypeInfo().Assembly.Location).ProductVersion;
                }
                return this._version;
            }
            set {
                this._version = value;
            }
        }
```
