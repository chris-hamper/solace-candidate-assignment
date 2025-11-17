# Discussion

## Remaining Bugs

1. Search input box and "searching for: ..." UI elements don't get cleared when the reset button is clicked. It's also not very maintainable. Reimplement the `#search-term` element and the HTML `<input>` component as a set of React components:
    - `<SearchBar>`: Parent component that holds the other components, and helps with layout. Accepts a `search` prop for the current value of the search term, and a `setSearch` function, and passes them along to other components as needed.
    - `<SearchingFor>`: Shows the current value of the `search` prop.
    - `<SearchInput>`: Renders the input containing the current search term, and calls `setSearch` whenever the value is changed.
    - `<SearchResetButton>`: When clicked, calls `setSearch` to clear the search text
    - Add state to the parent page to track the value of the search term, and pass the value and `set` function to `<SearchBar>`
    - Get rid of any unnecessary debugging `console.log()` calls.

2. Use explicit TypeScript typing throughout, to help avoid bugs creeping in later.

## Performance Improvements

1. Shift search filtering to backend:

    - Add a new, optional `search` query param to the endpoint that accepts a string
    - Update the DB query to only return results that are `ILIKE` the search string when specified (long-term consider using something like Elasticsearch instead of SQL if performance degrades)

2. Add pagination to `/api/advocates` endpoint:

    - Decide on a default sort order for results, to keep ordering consistent across requests. Update the DB query to use this sort.
    - Add optional `page` and `pageSize` query params that accept non-negative integers. Limit `pageSize` to 100. Default `pageSize` to 25.
    - Update the DB query to use `OFFSET` and `LIMIT` clauses based on the values of `page` and `pageSize`.

3, Add pagination components to UI for the list of advocates.

    - Add "Previous page" and "Next page" buttons, and a drop-down that allows the user to select page sizes of (10, 25, and 100), defaulting to 25.
    - Add state to track the current page number and page size
    - Consider adding the values for `page` and `pageSize` to the browser URL, so users will see the same view when navigating back to the page later.
    - Pass the values for `page` and `pageSize` to the API based on the newly added state.

## Style/UX Improvements

1. General UI theme:

    - Make Buttons look like buttons
    - Color / shading for better visual separation between UI sections

2. The table is pretty plain, and is not responsive:

    - Figure out how to show the info more clearly on a mobile device.
    - Spend some time making the table prettier, consistent with other Solace assets
    - Is there a clearer way of showing multiple specialties, so rows' heights are more consistent? (maybe a truncation in the UI with: `and X others`?)
    - Format the phone number in `(XXX) XXX-XXXX` form

3. Consider adding a `<Suspend>` to the table so that users don't see a flash of the table with default column widths before the API data loads. Display a spinner or placeholder while loading, consistent with other pages.
