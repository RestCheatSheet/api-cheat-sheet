# API Design Cheat Sheet
1. Build the API with consumers in mind--as a product in its own right.
    * Not for a specific UI.
    * Embrace flexibility / tunability of each endpoint (see #5, 6 & 7).
    * Eat your own dogfood, even if you have to mockup an example UI.

1. Use the Collection Metaphor.
    * Two URLs (endpoints) per resource:
        * The resource collection (e.g. /orders)
        * Individual resource within the collection (e.g. /orders/{orderId}).
    * Use plural forms (‘orders’ instead of ‘order’).
    * Alternate resource names with IDs as URL nodes (e.g. /resources/{id}/resources/{id})
    * Keep URLs as short as possible. Preferably, no more-than three nodes per URL.

1. Use nouns as resource names (e.g. don’t use verbs in URLs).

1. Make resource representations meaningful.
    * “No Naked IDs!” No plain IDs embedded in responses. Use links and reference objects.
    * Design resource representations. Don’t simply represent database tables.
    * Merge representations. Don’t expose relationship tables as two IDs-represent meaningful resources.

1. Support filtering, sorting, and pagination on collections.

1. Support link expansion of relationships. Allow clients to expand the data contained in the response by including additional representations instead of links.

1. Support field projections on resources. Allow clients to reduce the number of fields that come back.

1. Use the HTTP method names to mean something:
    * POST - create and other non-idempotent operations.
    * PUT - update.
    * GET - read a resource or collection.
    * DELETE - remove a resource or collection.

1. Use HTTP status codes to be meaningful.
    * 200 - Success.
    * 201 - Created. Returned on successful creation of a new resource. Include a 'Location' header with a link to the newly-created resource.
    * 400 - Bad request. Data issues such as invalid JSON, etc.
    * 404 - Not found. Resource not found on GET.
    * 409 - Conflict. Duplicate data or invalid data state would occur.

1. Use ISO 8601 timepoint formats for dates in representations.

1. Use Content-Type negotiation to describe incoming request payloads.
    * For example, let's say your doing ratings, including a thumbs-up/thumbs-down and five-star rating. You have one route to create a rating: **POST /ratings**

    How do you distinguish the incoming data to the service so it can determine which rating type it is: thumbs-up or five star?
    
    The temptation is to create one route for each rating type: **POST /ratings/five_star** and **POST /ratings/thumbs_up**
    
    However, by using Content-Type negotiation we can use our same **POST /ratings** route for both types. By setting the *Content-Type* header on the request to something like **Content-Type: application/vnd.company.rating.thumbsup** or **Content-Type: application/vnd.company.rating.fivestar** the server can determine how to process the incoming rating data.

1. Evolution over versioning. However, if versioning, use Accept header instead of URL.

1. Consider Cacheability.