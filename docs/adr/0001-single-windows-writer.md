# One Windows computer owns operational writes at launch

The first release must support offline sales while preventing sales beyond available stock. One Windows computer will perform all sales, inventory, and cash writes; the web application will provide viewing and reporting. This avoids independent offline devices selling the same stock, at the cost of deferring web checkout and simultaneous operational terminals.

This decision revises the offline PWA checkout requirement in the original PRD. Multi-device writes require a new authority and stock-allocation decision before implementation.
