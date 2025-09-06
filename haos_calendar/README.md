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

## License

This repository is released under the MIT License.
