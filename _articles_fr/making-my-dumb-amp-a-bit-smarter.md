---
title: Making my dumb amp a bit smarter
---
Just a bit of context: I've decided to write a little bit about what I'm doing with Home Assistant as a way to reminisce about how and why I've done certain things in such a way. Maybe it could be useful to a bunch of people? I don't know.

Since I've moved, I've been slowly rebuilding my dashboard from the ground up using mostly (if not exclusively) [Bubble Card](https://github.com/Clooos/Bubble-Card).

A few months ago, I made my own card for controlling my Yamaha R-S300 amp using [custom:button-card](https://github.com/custom-cards/button-card)
![](/images/articles/Pasted image 20260214065138.png)


![](/images/articles/Pasted image 20260214062806.png)

This card uses my Broadlink RM Mini 3 IR blaster/remote (that i got for ~25 euros) to send IR commands to my "dumb" amp.
Once Broadlink integration is set up:

![](/images/articles/Pasted image 20260214062550.png)
I can use remote.learn_command in Developer Tools -> Action to learn IR commands for the buttons on my remote that are useful to me: I use power (on/off), sleep, Speakers A / Speakers B (I use two pairs of speakers set up in different areas: near my desk on one side, below my bed on the other), Mute (on/off), Volume Up, Volume Down and three audio inputs: TV (Tape), Desktop (Line 1), Music (Line 2): this is a Chromebox CN60 running Ubuntu Server that I use as a Music Assistant player (Sendspin client). (You don't need to know all that but i wanted to mention it anyway).

Here, select Broadlink RM Mini 3 as a target, choose a device name (i chose "amp"), a command name ("volume_down"), click on "Perform action", aim your original amp remote towards the RM Mini 3 and click on the button (volume down in this case).

![](/images/articles/Pasted image 20260214062834.png)

You can then test if the command works using remote.learn_command (i had to move my IR blaster closer to the amp because it was a bit too far and commands sometimes failed):

![](/images/articles/Pasted image 20260214063858.png)

Then, I made a script to run my command using remote.send_command, my device name (amp) and command name (amp_speakers_a here):

![](/images/articles/Pasted image 20260214064226.png)

I can now run the script as part of an automation (e.g. turn amp_speakers_b off if my presence sensor detects that I'm at my desk) or when i press a button on my dashboard. great!

Example of an automation that triggers a script:
```
alias: "[Amp] Switch to Line 2 if Philibert is playing"
description: ""
triggers:
  - trigger: state
    entity_id:
      - media_player.philibert
    to:
      - playing
conditions: []
actions:
  - action: script.turn_on
    metadata: {}
    target:
      entity_id: script.amp_line_2
    data: {}
mode: single

```

Example of a Bubble card (sub-buttons type) with Tap action: Perform action - Toggle Script

![](/images/articles/Pasted image 20260214073632.png)

![](/images/articles/Pasted image 20260214073649.png)


```type: custom:bubble-card
card_type: sub-buttons
button_type: name
sub_button:
  main: []
  bottom:
    - name: ""
      icon: mdi:power
      tap_action:
        action: perform-action
        perform_action: script.toggle
        target:
          entity_id: script.amp_power
      content_layout: icon-left
      fill_width: false
slider_fill_orientation: left
slider_value_position: right
name: Power
icon: mdi:power
tap_action:
  action: perform-action
  perform_action: script.toggle
  target:
    entity_id: script.amp_power
button_action: {}
rows: 0.938

```

Example of a custom button card:

```type: custom:button-card
color_type: card
color: rgb(210, 166, 172)
icon: mdi:power
tap_action:
  action: call-service
  service: script.amp_power
```



I first achieved that using Input buttons, but I realized later on that Input booleans were more powerful: if I only use Home Assistant to control my amp, I can track the on/off state of my speakers, the current source, and so on.

It might be more complicated than you'd need (if you plan on using your original remote along with Home Assistant, you can use input buttons for everything), but this is what I ended up with:
- Input boolean for each of my three sources (Tape, Line 1, Line 2)
- Mute - input boolean
- Power - input boolean
- Sleep - input button (because it uses more than two states: 90 minutes, 60 minutes, 30 minutes, off… and also because i'll probably never bother with this button anyway)
- Speakers A - input boolean
- Speakers B - input boolean
- Volume Down - input button
- Volume Up - input button
- Amp Source - input select with three options:  TV, PC, Music

![](/images/articles/Pasted image 20260214074044.png)

- Amp Volume - input number

![](/images/articles/Pasted image 20260214072303.png)

If the amp volume helper and the actual volume are out of sync, you can just type the actual value to update it.

I use these last two helpers to display the current volume and source:

![](/images/articles/Pasted image 20260214074331.png)

I used conditional styling to change the gradient colors depending on the current source:

![](/images/articles/Pasted image 20260214074548.png)


Full code for my "final" card:

```
type: vertical-stack
cards:
  - type: custom:bubble-card
    card_type: button
    button_type: state
    sub_button:
      main:
        - sub_button_type: default
          entity: input_number.amp_volume
          state_background: false
          show_attribute: false
          show_last_changed: false
          show_state: true
          show_last_updated: false
          tap_action:
            action: none
          double_tap_action:
            action: none
          content_layout: icon-left
        - entity: input_select.amp_source
          sub_button_type: default
          show_last_changed: false
          show_state: true
          tap_action:
            action: none
          double_tap_action:
            action: none
          content_layout: icon-left
      bottom: []
    entity: input_select.amp_source
    name: R-S300
    icon: mdi:amplifier
    show_state: false
    force_icon: false
    show_last_changed: false
    show_attribute: false
    tap_action:
      action: none
    button_action:
      tap_action:
        action: none
    styles: |-
      * {
          --bc-icon-bg: #414144;
        }

        .bubble-name {
          color: #ffffff !important;
          font-size: 18px !important;
        }

        .bubble-sub-button-1,
        .bubble-sub-button-2 {
          font-size: 14px !important;
          background-color: #414144 !important;
        }

        .bubble-sub-button-icon {
          --mdc-icon-size: 22px !important;
        }

        .bubble-content-container::before {
          background: ${(() => {
            if (hass.states['input_boolean.amp_tv_tape_toggle']?.state === 'on') {
              return `radial-gradient(circle at -10% 0%, #c4c1e0 -40%, #88bdbc 60%)`;
            }

            if (hass.states['input_boolean.amp_desktop_line_1_toggle']?.state === 'on') {
              return `radial-gradient(circle at -10% 0%, #d2a6ac -40%, #c4c1e0 60%)`;
            }

            if (hass.states['input_boolean.amp_music_line_2_toggle']?.state === 'on') {
              return `radial-gradient(circle at -10% 0%, #88bdbc -40%, #c4c1e0 60%)`;
            }

            return `radial-gradient(circle at -10% 0%, #555555 -40%, #888888 60%)`;
          })()} !important;
        }
    card_layout: large
    rows: 1
    modules:
      - "!default"
      - bubble_neon
    slider_fill_orientation: left
    slider_value_position: right
    scrolling_effect: false
  - type: custom:bubble-card
    card_type: sub-buttons
    sub_button:
      main: []
      bottom:
        - buttons_layout: inline
          group:
            - show_state: false
              show_last_changed: false
              state_background: true
              tap_action:
                action: perform-action
                perform_action: script.amp_power
                target: {}
              content_layout: icon-left
              entity: input_boolean.amp_power_toggle
              name: Power
              fill_width: true
            - entity: input_boolean.amp_speakers_a_toggle
              tap_action:
                action: perform-action
                perform_action: script.amp_speakers_a_2
                target: {}
              content_layout: icon-left
              name: Speakers A
              icon: mdi:alpha-a-circle
            - entity: input_boolean.amp_speakers_b_toggle
              tap_action:
                action: perform-action
                perform_action: script.amp_speakers_b
                target: {}
              content_layout: icon-left
              show_last_updated: false
              name: Speakers B
              icon: mdi:alpha-b-circle
            - tap_action:
                action: perform-action
                perform_action: script.amp_sleep
                target: {}
              entity: input_boolean.amp_sleep_toggle
              name: Sleep
          name: First row
      bottom_layout: rows
    footer_mode: false
    rows: 0.938
    styles: |-
      .bubble-sub-button-1 {
          color: ${
            hass.states['input_boolean.amp_power_toggle'].state === 'on'
              ? '#414144'
              : 'default'
          } !important;
          background-color: ${
            hass.states['input_boolean.amp_power_toggle'].state === 'on'
              ? '#88bdbc'
              : 'default'
          } !important;
        }
      .bubble-sub-button-2 {
          color: ${
            hass.states['input_boolean.amp_speakers_a_toggle'].state === 'on'
              ? '#414144'
              : 'default'
          } !important;
          background-color: ${
            hass.states['input_boolean.amp_speakers_a_toggle'].state === 'on'
              ? '#d2a6ac'
              : 'default'
          } !important;
        }
      .bubble-sub-button-3 {
          color: ${
            hass.states['input_boolean.amp_speakers_b_toggle'].state === 'on'
              ? '#414144'
              : 'default'
          } !important;
          background-color: ${
            hass.states['input_boolean.amp_speakers_b_toggle'].state === 'on'
              ? '#c4c1e0'
              : 'default'
          } !important;
        }
      .bubble-sub-button-4 {
          color: ${
            hass.states['input_boolean.amp_sleep_toggle'].state === 'on'
              ? '#414144'
              : 'default'
          } !important;
          background-color: ${
            hass.states['input_boolean.amp_sleep_toggle'].state === 'on'
              ? '#c4c1e0'
              : 'default'
          } !important;
        }
      .bubble-sub-button-icon {
          --mdc-icon-size: 32px !important;
        }
  - type: custom:bubble-card
    card_type: sub-buttons
    card_layout: large-sub-buttons-grid
    sub_button:
      main: []
      bottom:
        - name: Group 1
          buttons_layout: inline
          group:
            - entity: input_button.amp_volume_down
              tap_action:
                action: perform-action
                perform_action: script.amp_volume_down
                target: {}
              content_layout: icon-left
              name: Volume Down
            - tap_action:
                action: perform-action
                perform_action: script.amp_mute
                target: {}
              entity: input_boolean.amp_mute_toggle
              content_layout: icon-left
              fill_width: false
              name: Mute
            - entity: input_button.amp_volume_up
              tap_action:
                action: perform-action
                perform_action: script.amp_volume_up
                target: {}
              name: Volume Up
      bottom_layout: rows
    styles: |
      .bubble-sub-button-2 {
          color: ${
            hass.states['input_boolean.amp_mute_toggle'].state === 'on'
              ? '#414144'
              : 'default'
          } !important;
          background-color: ${
            hass.states['input_boolean.amp_mute_toggle'].state === 'on'
              ? '#88bdbc'
              : 'default'
          } !important;
        }
      .bubble-sub-button-icon {
          --mdc-icon-size: 28px !important;
        }
    rows: 0.938
  - type: custom:bubble-card
    card_type: sub-buttons
    button_type: switch
    sub_button:
      main: []
      bottom:
        - name: Sources
          buttons_layout: inline
          group:
            - entity: input_boolean.amp_tv_tape_toggle
              tap_action:
                action: perform-action
                perform_action: script.amp_tape
                target: {}
              name: TV
            - entity: input_boolean.amp_desktop_line_1_toggle
              tap_action:
                action: perform-action
                perform_action: script.amp_line_1
                target: {}
              name: Desktop
            - entity: input_boolean.amp_music_line_2_toggle
              tap_action:
                action: perform-action
                perform_action: script.amp_line_2
                target: {}
              content_layout: icon-left
              name: Music
    rows: 0.938
    styles: |-
      .bubble-sub-button-1 {
          color: ${
            hass.states['input_boolean.amp_tv_tape_toggle'].state === 'on'
              ? '#414144'
              : 'default'
          } !important;
          background-color: ${
            hass.states['input_boolean.amp_tv_tape_toggle'].state === 'on'
              ? '#88bdbc'
              : 'default'
          } !important;
        }
      .bubble-sub-button-2 {
          color: ${
            hass.states['input_boolean.amp_desktop_line_1_toggle'].state === 'on'
              ? '#414144'
              : 'default'
          } !important;
          background-color: ${
            hass.states['input_boolean.amp_desktop_line_1_toggle'].state === 'on'
              ? '#d2a6ac'
              : 'default'
          } !important;
        }
      .bubble-sub-button-3 {
          color: ${
            hass.states['input_boolean.amp_music_line_2_toggle'].state === 'on'
              ? '#414144'
              : 'default'
          } !important;
          background-color: ${
            hass.states['input_boolean.amp_music_line_2_toggle'].state === 'on'
              ? '#c4c1e0'
              : 'default'
          } !important;
        }
      .bubble-sub-button-icon {
          --mdc-icon-size: 32px !important;
        }

```