# Perfume Retail

The language of store operations, sales, and inventory for Perfume POS.

## Language

**Sellable presentation**:
A specific perfume offering sold and counted as discrete units, with its own inventory identity. A 100 ml bottle and a prefilled 5 ml decant are separate sellable presentations.
_Avoid_: Product when referring specifically to a stocked presentation

**Decant**:
A smaller portion of fragrance packaged for sale separately from its source bottle. In the first release, decants enter inventory as finished, prefilled units.

**Pilot**:
A supervised trial of the application in one real store before general production use.

**Return**:
The return of items from a completed sale, linked to the original sale and its items. Returned items are classified as resellable or damaged.

**Refund**:
Money returned to a customer against an original sale. Its payment method is recorded separately from the disposition of returned stock.

**Cash session**:
A period of register responsibility owned by one cashier, starting with an opening cash balance and ending with a counted close.
_Avoid_: Shift when referring specifically to register accountability

**Goods receipt**:
A confirmed arrival of finished sellable presentations into store inventory, recording quantities and acquisition costs.
_Avoid_: Purchase when referring only to the arrival of stock

**Moving weighted-average cost**:
The average acquisition cost of available units, recalculated when stock is received using existing inventory value and the value received.

**Product**:
A catalog grouping of related sellable presentations. Stock belongs to each presentation rather than to the product grouping.

**Suspended cart**:
An unfinished checkout saved for later without reserving stock. Prices and availability are checked again when it is resumed.

**Exchange**:
A linked return and new sale, each preserving its own stock and payment effects.

**Recovery gap**:
The period of store activity that cannot be recovered from either a backup or synchronized records and must be reconciled before checkout resumes.
