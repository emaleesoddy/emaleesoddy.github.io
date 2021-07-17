## understanding feed publisher & feed types in salesforce lightning communities
<small>originally written & published 12.06.2017</small>

>This article was written for the [Summer '17 Salesforce Release](https://releasenotes.docs.salesforce.com/en-us/summer17/release-notes/salesforce_release_notes.htm). In future releases, some or all of the information below may be inaccurate.

This article is the documentation I wish existed for the various “Feed” components in Salesforce Lightning Communities. I have collected this information mainly through trial-and-error, first-hand experiences while building communities for enterprise clients. If you have additions to this information or answers to any of my open questions, please let me know in the discussion section below.

### Feed Publisher

* [Salesforce Documentation](https://developer.salesforce.com/docs/atlas.en-us.community_templates.meta/community_templates/rss_feed_publisher.htm)

The most important property of the Feed Publisher to understand is the `Type` field. `Type` refers to the Publisher Layout you wish to show, `Global` or `Record`. These can be modified in Salesforce Setup, but it appears that the Feed Publisher component will not allow you to select custom Publisher Layouts from the dropdown.

It is not possible to post to the entire community from the Feed Publisher. Below are the target audiences for the publisher, depending on what page type you are on:

* **Standard Pages:** "To My Followers"
* **Topic Detail:** "To {Topic}"
* **Group Detail:** "To {Group}"
* **User Profile:**<sup>✝</sup> "To {User}"
* **Record Detail:** "To {Company} Only," or "To all with access".

<sup>✝</sup><small>Note that this does not mean the post is private to the user. Anyone that has access to view the user’s profile will see your message. If you’re looking for a way to allow users to privately message one another, you may want to [enable Direct Messaging](https://help.salesforce.com/articleView?id=networks_enable_direct_messages.htm&type=0&language=en_US&release=208.8) in your community.</small>

For more detail on “Who Sees What” when you post to Chatter, take a look at [this section in Trailhead’s “Lightning Experience Chatter Basics” module](https://trailhead.salesforce.com/en/modules/chatter/units/chatter_intro#Tdxn4tBK-heading11).

### Feed & Feed Compact
* [Salesforce Documentation](https://developer.salesforce.com/docs/atlas.en-us.community_templates.meta/community_templates/rss_feed.htm) (Feed)
* [Salesforce Documentation](https://developer.salesforce.com/docs/atlas.en-us.community_templates.meta/community_templates/rss_feed_compact.htm) (Feed Compact)

As of Summer '17, there is no such thing as a truly open/public feed. In most cases, Feeds work similarly to Facebook. Users will need to follow people and topics that interest them— if they are not following anything, their feed is most likely empty or shows only the feed content they have personally created in the community.

Once again, the most important property of the Feed & Feed Compact components to understand is the `Type` field. Below is a breakdown of what type of content, or “feed item” shows when the different options available for the `Type` property are selected.

* **Group or Record**
  * Shows all feed items created on a specific record or within a specific group.
* **Community Discussion**
  * Shows all feed items tagged with a navigational topic; users do not need to be following the navigational topic, and feed items without navigational topics will not appear even if the user is following that topic.
  * Shared feed items will only appear in this feed if the message accompanying the shared item includes #{topic}, where {topic} is a navigational topic. Even if the original feed item had a navigational topic assigned, it will not appear in the feed if the navigational topic is not included in the share message with the hashtag notation.
* **My Feed**
  * Shows feed items tagged with any topic and/or person the user is following. This type will also show all of the user’s personal activity in the community.
  * Users will see all shared feed items unless the item was shared with a group they are not a member of by a different user. Users are able to share feed items to public groups they are not members of; in this case, they will see the feed item they shared to the group despite not being a member.
* **Topic**
  * Shows all feed items with the specific topic.
  * Shared feed items will only appear in this feed if the message accompanying the shared item includes #{topic}, where {topic} is the specific topic. Even if the original feed item had the topic assigned, it will not appear in the feed if the topic is not included in the share message with the hashtag notation.
* **User Profile**
  * Shows the user’s personal activity.
* **Bookmarks**
  * Shows feed items the user has bookmarked.

### Resources
* [Trailhead: Chatter Basics](https://trailhead.salesforce.com/modules/chatter)
* [Trailhead: Lightning Experience Chatter Basics](https://trailhead.salesforce.com/en/modules/lex_implementation_chatter)

If you would like to see a public feed option eventually make its way to Lightning Communities, be sure to [vote up the idea](https://success.salesforce.com/ideaView?id=0873A0000003SbzQAE) in Salesforce's Success Community!