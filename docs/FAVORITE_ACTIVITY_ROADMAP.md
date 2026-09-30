# Favorite Activity roadmap

Build 15 (`0bba199b`) is the current known-good Windows baseline.

## Next implementation milestones

### 1. Jump to live
- Reuse `ChannelView::goToBottom_` rather than adding a competing overlay.
- Rename the Favorite Activity presentation to `↓ Jump to latest messages`.
- Synced mode: clicking the control in either activity view sends both views to bottom/live.
- Unlinked mode: clicking affects only the selected view.
- Preserve the existing smooth-scroll preference.

### 2. Live clock
- Add an `HH:MM` local-time clock to activity-bearing Chatterino windows.
- Cover normal split/chat activity, Favorite Activity, and individual user activity/user-card views.
- Update on the minute boundary rather than polling every second.
- Keep placement compact and theme-aware.

### 3. Activity-event coverage
Verify Favorite Activity attribution for more than ordinary chat messages:
- normal messages, replies, and `/me`
- subscriptions and resubscriptions
- gifted subscriptions / gifter identity when Twitch supplies it
- Cheers/Bits
- channel-point redemptions
- announcements
- relevant moderation activity
- other user-associated events Chatterino places on the channel timeline

Do not synthesize events that Chatterino/Twitch did not receive. Preserve blank rows for nonmatching timeline entries so synchronized alignment remains exact.

### 4. Tracked phrases / keywords
Allow multiple phrases/keywords to qualify a row for Favorite Activity even when the author is not a favorite.

Planned rule options:
- enabled/disabled per rule
- case-sensitive or case-insensitive
- whole-word or contains matching
- phrase matching
- add/edit/remove rules
- visible reason marker, e.g. `★ Favorite User` or `🔎 Keyword: phrase`

A row is shown when it matches a favorite user OR any enabled tracked phrase rule. It must retain its original timeline index/height so sync remains exact.

### 5. Favorite Activity QoL
- favorite count/status in header
- empty state when no users/rules are configured
- compact favorite/rule manager
- optional visual markers for why an item matched
- immediate repaint when favorites/rules change
- keep pane resize, close/hide, width persistence, and Synced/Unlinked persistence

## Stability rule
Do not regress the Build 15 startup initialization fix. Favorite Activity pointer/state members must be initialized before `SplitHeader` can query Favorite Activity visibility. Keep Crashpad and matching PDB artifacts enabled for Windows test builds.
