```mermaid
graph TD
  subgraph Arc1 [Story Arc A]
    A1[Event A1] --> A2[Event A2]
  end
  subgraph Arc2 [Story Arc B]
    B1[Event B1] --> B2[Event B2]
  end
  A2 --> B1
