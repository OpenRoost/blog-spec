# Blog Site Learning Progression

This document defines a framework-neutral implementation sequence for the Blog
Site educational project. The project specification in
[`index.md`](./index.md) is authoritative for product behavior; this document
organizes that behavior into manageable learning milestones.

A course may add framework-specific exercises, such as converting handler
functions into class-based handlers or using a particular ORM, template engine,
or serialization library. Such exercises are course material and are not
canonical Blog Site requirements.

## Milestone 1: HTTP routing

Create the web application's required routes and connect them to request
handlers. Placeholder responses are acceptable at this stage.

### Acceptance criteria

- Every core web path from the specification can be requested using its
  required HTTP method.
- Dynamic route parameters are accepted where the specification requires them.
- Unknown paths produce an appropriate not-found response.
- Routes that are intended to change persistent state are not implemented as
  `GET`-only operations.

## Milestone 2: Domain model and persistence

Implement persistent storage for the core domain entities and their
relationships.

### Acceptance criteria

- Topics, articles, comments, and users can be persisted and retrieved.
- An article has one author and may be associated with multiple topics.
- A comment belongs to one article and has one author.
- Creation and update timestamps behave according to the specification.
- Invalid relationships or uniqueness constraints are rejected rather than
  silently persisted.
- Restarting the application does not lose previously persisted project data.

## Milestone 3: Server-rendered interface

Replace placeholder responses with the server-rendered pages described by the
specification.

### Acceptance criteria

- The homepage renders an article list.
- Article detail pages render article metadata, content, topics, and comments.
- Public profile pages render user information and authored articles.
- Common navigation allows users to reach the primary application pages.
- Forms required by later milestones have usable rendered pages.
- User-controlled content is rendered safely rather than injected as trusted
  HTML.

## Milestone 4: CRUD and validation

Make article and comment forms operate on persistent data.

### Acceptance criteria

- A valid article submission creates an article.
- An existing article can be updated without replacing its original creation
  timestamp.
- Article deletion requires an explicit user action and removes the selected
  article.
- A valid comment submission creates a comment related to the selected
  article.
- An article cannot be created without the required fields and topics.
- Invalid submissions present useful validation feedback.

## Milestone 5: Authentication

Implement user registration and authenticated sessions or an equivalent
framework mechanism.

### Acceptance criteria

- A new user can register with valid unique credentials.
- Duplicate usernames and mismatched password confirmation are rejected.
- A registered user can authenticate with valid credentials.
- Invalid credentials do not authenticate a user.
- An authenticated user can log out and return to an anonymous state.
- Protected pages can redirect an anonymous user to authentication and return
  the user to the original destination afterwards.

## Milestone 6: Authorization

Apply ownership and user-access rules to existing functionality.

### Acceptance criteria

- Anonymous users cannot create articles or comments.
- Authenticated users can create articles and comments as themselves.
- An article author can update and delete their own article.
- Another regular authenticated user cannot update or delete that article.
- Users can modify only their own profile, password, preferences, and
  subscription state.
- Account deactivation prevents future authentication while preserving
  authored content.

## Milestone 7: Preferences and subscriptions

Implement topic preferences and notification-subscription state.

### Acceptance criteria

- An authenticated user can add and remove preferred topics.
- Preference state is isolated per user.
- Notification subscription can be enabled and disabled for a preferred topic.
- Removing a preferred topic also removes any subscription state associated
  with that preference.
- Anonymous users cannot modify preference or subscription state.

## Milestone 8: REST API

Expose the core Blog Site behavior through the REST API contract defined by the
project specification.

### Acceptance criteria

- Public resources can be retrieved without authentication where permitted.
- Registration and authentication are available through the API.
- Authenticated users can create articles and comments.
- Article ownership rules are enforced for API update and delete operations.
- Private profile and preference data is available only to the appropriate
  authenticated user, except for explicitly permitted administrative access.
- Validation, authentication, authorization, and not-found failures use
  appropriate HTTP responses.
- Client-provided user identifiers cannot impersonate the authenticated user.

## Milestone 9: Optional extensions

Optional extensions are independent exercises. A course may require any subset
of them.

### Archive

Provide article archive pages by year and month.

Acceptance criteria:

- Valid year/month combinations return the matching article collection.
- Invalid month values are rejected.
- Both one-digit and zero-prefixed month forms may be supported as specified.

### Article slugs

Use human-readable article slugs in detail URLs.

Acceptance criteria:

- Every article receives a unique slug.
- Slug generation is deterministic according to the project specification.
- Existing articles receive valid slugs when the feature is introduced.
- Article detail lookup works through the slug.

### Personalized article ordering

Use topic preferences as an additional article-ordering signal.

Acceptance criteria:

- Anonymous users retain the default ordering.
- Authenticated users receive ordering influenced by their preferred topics.
- Publication recency keeps the precedence defined by the project
  specification.

### Account reactivation

Allow a deactivated user to request restoration of account access.

Acceptance criteria:

- Reactivation does not bypass account-owner verification.
- A successful reactivation restores authentication without losing authored
  content.

### Article likes

Allow authenticated users to express a simple reaction to an article.

Acceptance criteria:

- An authenticated user can like and unlike an article.
- A user can contribute at most one active like to a given article.
- The visible article like count reflects the current number of active likes.
- Anonymous users may read the like count but cannot change it.
- The persistence mechanism for likes is an implementation choice; no generic
  relation or framework-specific model pattern is required.
