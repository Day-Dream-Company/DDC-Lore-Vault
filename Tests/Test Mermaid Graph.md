```mermaid
graph TD
  %% Branch A
  A1["A1"] --> A2["A2"] --> A3["A3"]

  %% Branch B
  B1["B1"] --> B2["B2"]

  %% Branch C
  C1["C1"] --> C2["C2"]

  %% Divergence
  A1 --> B1
  A1 --> C1

  %% Merge
  B2 --> A3
  C2 --> A3

