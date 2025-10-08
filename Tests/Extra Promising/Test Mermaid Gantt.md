```mermaid
gantt
    title Example Timeline
    dateFormat  YYYY-MM-DD
    %% optional: exclude weekends or specific dates
    excludes    weekends

    section Group A
    Event A1 :a1, 2024-01-01, 5d
    Event A2 :after a1, 3d

    section Group B
    Event B1 :b1, 2024-01-02, 7d
    Event B2 :after b1, 4d
