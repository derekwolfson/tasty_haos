# Home Assistant Dashboard YAML

This repository contains a custom Home Assistant dashboard configuration that integrates multiple calendars, weather information, and flexible views (daily, 2-week, monthly). The dashboard uses `custom:week-planner-card` and `custom:button-card` for advanced interactivity and styling.

NOTE: This was written with ChatGPT -- so it may not make any sense.  I have not proof read it.
---

## Features

- **Home View**: A welcoming markdown header and real-time weather information including temperature, humidity, UV index, and wind speed.
- **Calendar Views**: Switch between Daily, 2-Week, and Monthly views.
- **Responsive Layout**: Adjusts for desktop and mobile screens.
- **Legend and Event Styling**: Custom CSS for headers, legends, day blocks, and events.
- **Sticky Navigation**: Calendar view tabs remain accessible at the top of the page.

---

## Dashboard Structure

### Home View

```yaml
views:
  - title: Home
    sections: []
    header:
      card:
        type: markdown
        text_only: true
        content: |-
          # Welcome Home
          What's going on? ✨
    badges:
      - type: entity
        show_name: false
        show_state: true
        show_icon: true
        entity: weather.forecast_home
        state_content:
          - temperature
          - humidity
          - uv_index
          - wind_speed
```

### Calendar Sections

```yaml
  - type: sections
    max_columns: 4
    title: Calendars
    path: calendars
    icon: ''
    subview: false
```

### Calendar Buttons

```yaml
      - type: grid
        cards:
          - type: vertical-stack
            cards:
              - type: horizontal-stack
                cards:
                  - type: custom:button-card
                    name: Daily
                    tap_action:
                      action: call-service
                      service: input_select.select_option
                      service_data:
                        entity_id: input_select.calendar_subtab
                        option: daily

                  - type: custom:button-card
                    name: 2 Week
                    tap_action:
                      action: call-service
                      service: input_select.select_option
                      service_data:
                        entity_id: input_select.calendar_subtab
                        option: 2 week

                  - type: custom:button-card
                    name: Monthly
                    tap_action:
                      action: call-service
                      service: input_select.select_option
                      service_data:
                        entity_id: input_select.calendar_subtab
                        option: monthly
```

### Calendar Cards (Week Planner)

#### Daily View

```yaml
  - type: conditional
    conditions:
      - entity: input_select.calendar_subtab
        state: daily
    card:
      type: custom:week-planner-card
      calendars:
        - entity: calendar.icmw_derek
          name: Derek-Work
          icon: mdi:briefcase
          color: '#4ECDC4'
        - entity: calendar.childcard_schedule
          name: Childcare
          icon: mdi:teddy-bear
          color: '#FF6B6B'
        - entity: calendar.quinn
          name: Quinn
          icon: mdi:account
      days: 1
      startingDayOffset: 0
      compact: true
      showLegend: true
      showNavigation: true
      dayFormat: '<big><b>%d</b></big>'
```

#### 2-Week View

```yaml
  - type: conditional
    conditions:
      - entity: input_select.calendar_subtab
        state: 2 week
    card:
      type: custom:week-planner-card
      calendars:
        - entity: calendar.icmw_derek
          name: Derek-Work
          icon: mdi:briefcase
          color: '#4ECDC4'
      days: 14
      startingDayOffset: 0
      compact: false
      showLegend: true
      showNavigation: true
```

#### Monthly View

```yaml
  - type: conditional
    conditions:
      - entity: input_select.calendar_subtab
        state: monthly
    card:
      type: custom:week-planner-card
      calendars:
        - entity: calendar.icmw_derek
          name: Derek-Work
          icon: mdi:briefcase
          color: '#4ECDC4'
      days: 30
      startingDayOffset: 0
      compact: false
      showLegend: true
      showNavigation: true
```

---

## Notes

- Make sure the custom cards `custom:week-planner-card` and `custom:button-card` are installed in Home Assistant via HACS or manually.
- The dashboard assumes an `input_select` called `calendar_subtab` exists with options `daily`, `2 week`, and `monthly`.
- You can adjust `days` and `startingDayOffset` to control the displayed range of the calendar.
- The `dayFormat` property can be customized for larger or styled day numbers.

---

# Home Assistant Monthly Calendar Dashboard

This repository contains a Home Assistant dashboard configuration using the `custom:week-planner-card`. It displays a monthly calendar with multiple family and work calendars, styled with custom CSS for readability, navigation, and event highlighting.

---

## Features

- **Multiple Calendars**: Displays individual calendars for Derek, children, family events, and more.
- **Monthly View**: Shows all 31 days of the month.
- **Compact Layout**: Optimized for smaller screens while maintaining readability.
- **Legend and Title**: Includes a toggleable legend and centered title.
- **Navigation Controls**: Navigate between months easily.
- **Responsive CSS Grid**: Sunday-first layout with styled day blocks and events.
- **Highlight Today**: The current day is visually distinguished.
- **Custom Styling**: Includes detailed CSS for headers, navigation, day blocks, and events.

---

## Calendars Included

| Calendar | Icon | Color |
|----------|------|-------|
| Derek-Work | mdi:briefcase | #4ECDC4 |
| Childcare | mdi:teddy-bear | #FF6B6B |
| Quinn | mdi:account-cowboy-hat | orange |
| Sawyer | mdi:unicorn-variant | pink |
| Family Events | mdi:human-male-female-child | #96CEB4 |

---

## Dashboard YAML Overview

```yaml
type: custom:week-planner-card
calendars:
  - entity: calendar.icmw_derek
    name: Derek-Work
    icon: mdi:briefcase
    color: "#4ECDC4"
  - entity: calendar.childcard_schedule
    name: Childcare
    icon: mdi:teddy-bear
    color: "#FF6B6B"
  - entity: calendar.quinn
    name: Quinn
    icon: mdi:account-cowboy-hat
    color: orange
  - entity: calendar.sawyer
    name: Sawyer
    icon: mdi:unicorn-variant
    color: pink
  - entity: calendar.family_events
    name: Family
    icon: mdi:human-male-female-child
    color: "#96CEB4"
days: "31"
startingDayOffset: 0
hideWeekend: false
noCardBackground: false
compact: true
showLegend: true
legendToggle: true
startingDay: month
showNavigation: true
grid_options:
  columns: full
dateFormat: DD
timeFormat: t
texts:
  fullDay: All-Day
  moreEvents: More...
  noEvents: " "
showTitle: true
showDescription: false
dayFormat: <big><big><b>dd</b> | </big>ccc</big>
visibility:
  - condition: state
    entity: input_select.calendar_subtab
    state: monthly
  - condition: not
    conditions:
      - condition: screen
        media_query: "(min-width: 0px) and (max-width: 767px)"
title: Monthly Calendar
card_mod:
  style: |
    /* === HEADER STYLING === */
    .header { ... }
    .navigation { ... }
    .legend { ... }
    /* === CSS GRID FOR DAYS === */
    .container { ... }
    .day { ... }
    .day.today { ... }
    .day .event { ... }
```

> The `card_mod` section contains detailed CSS for header, navigation, legend, day blocks, and events. It ensures readability, spacing, and highlights the current day.

---

## Custom CSS Notes

- **Header**: Centered text with top and bottom dividers.
- **Navigation**: Flex layout for month navigation and arrow icons.
- **Legend**: Wraps events legend, centers text, and adjusts spacing.
- **Days Grid**: 7-column layout with Sunday as the first column.
- **Day Blocks**: Minimum height, rounded corners, inner wrapper for events.
- **Events**: Inline-flex layout, small icons, word wrapping, padding.
- **Today Highlight**: Background and bold date font.

---

## Requirements

- `custom:week-planner-card` installed via HACS or manually.
- `input_select.calendar_subtab` with option `monthly`.
- Home Assistant with Lovelace dashboard support.
- `card_mod` integration installed to apply the CSS.

---

## License

MIT License


## License

This repository is released under the MIT License.
