slack-lunch
===========


### Architecture

* node.js app
* lives on heroku
* assumptions:
  * slack has a user named @lunchbot
  * slack has a channel named #lunchspiration

### User Flow

At 11:00am, `@lunchbot` invites `#general` to participate.

At 11:30am, if there are at least 2 participants, `@lunchbot` DMs participants their initial lunch match.

At noon, `@lunchbot` handles cancellations, reshuffles, and DMs new lunch matches as necessary.

Matching algorithm:

* Only 1 participant?
  * Wait.
* Else
  * An even number of participants?
    * Pair them off evenly.
  * Else
    * Create a group of 3.


### Copy

At 11:00am on a workday, `@lunchbot` posts to `#general`:

    lunchbot 11:00am
    Hey @channel! Getting hungry? If you DM me food emoji in the next 30 minutes,
    I will match you with a colleague and suggest a tasty place to eat by noon.

When `@user` DMs `@lunchbot` before noon on a workday:

    user 11:30am
    [food emoji]

    lunchbot 11:30am
    Howdy, <@user>! Hang tight while I cook up your lunch plans.

When `@user` DMs `@lunchbot` after noon on a workday:

    user 12:01pm
    [food emoji]

    lunchbot 12:01pm
    Aw shucks, <@user>, I've already paired folks up today. But no need to eat alone:
    join #lunchspiration to see everywhere I've sent folks to today. Pick a place
    on the list and I bet you'll find someone you know. Bon apetit!

When `@user` DMs `@lunchbot` at any time on a non-workday:

    user 3:32am
    [food emoji]

    lunchbot 3:32am
    Howdy, <@user>! I can't pair you up today, but join #lunchspiration
    for lunchspiration anytime.


#### Match Flow



If there is a cancellation after the final reshuffle, `@lunchbot` sends DM:

    lunchbot 12:01pm
    Aw, bummer, @userB can't make it. But no need to eat alone:
    join #lunchspiration to see everywhere I've sent folks to today. Pick a place
    on the list and I bet you'll find someone you know. Bon apetit!

If there is a cancellation after the initial reshuffle, `@lunchbot` sends DM:

    lunchbot 11:36pm
    Aw, bummer, @userB can't make it. No worries, I'm reshuffling things around
    and will match you up with someone new by noon.


At 11:45am, `@lunchbot` sends DM to `<@userA>:

    lunchbot 11:45am
    Woohoo, lunchtime! @userA, you get to have lunch with @userB today!
    I'm sending you to <restaurant> <yelp link>.
    Are you in? Say 'yes'!

    (Psst: Can't make it after all?
    Tell me 'no' right away so I can match @userB with someone else.)

At 11:45am, in DM to @userB:

    lunchbot 11:45am
    Yay, lunchtime! @userB, you get to have lunch with @userA today!
    I'm sending you to <restaurant> <yelp link>.
    Are you in? Say 'yes'!
    
    (Psst: Can't make it after all?
    Tell me 'no' right away so I can match @userA with someone else.)

If the user says 'yes':

    lunchbot 11:47am
    Rad! Go find @user. Here's how to get to <restaurant>: <directions>. Bon appetit!

If the user says 'no':

    lunchbot 11:47am
    No worries, things come up. I'll match @user up with someone else, and we'll
    try again <next day with food>. Good luck with the rest of your day!


#### Review Flow

At 2pm, in DM to all users who participated:

    lunchbot 2:00pm
    Did you end up eating at <restaurant>?

    [if no]
    
    No worries! Thanks for playing lunchmatch.

    [if yes]
    
    Would you eat there again?
    
    [if no]
    
    Bummer! I won't send you there again.
    
    [if yes]



    lunchbot 2:00pm
    Describe lunch at <restaurant> in emoji form.

Would you eat there again?
    
    How was lunch? Answer in emoji form.




At 2pm, in `#lunchroulette`:
    @lunchbot
    @userA Would you go to <restaurantA> again? :thumbsup: or :thumbsdown: 
    @userB Would you go to <restaurantA> again?  :thumbsup: or :thumbsdown: 


Slack bot implementation of LunchRoulette


https://github.com/codeforamerica/lunch_roulette


https://github.com/andrewchilds/slack-notify
