# Bruno MCP by incwo

Public connection guide and server metadata for the remote Bruno MCP connector.

Bruno connects an existing Bruno account to an MCP client so you can work with your authorized companies, colleagues, agents, conversations, missions and files. Availability depends on your account, current permissions, credits and connected services.

## Connection

Add a remote MCP connector in your client with this server URL:

```text
https://bruno.incwo.com/api/platform/v1/mcp
```

Choose OAuth with automatic client registration when your client offers that choice. Sign in to Bruno at the Bruno authorization page, review the requested permissions and approve the connection. Your existing company and agent permissions continue to apply to each tool call.

The general endpoint is intended for clients requiring the broader Bruno feature set. Directory profiles are documented separately below. Directory availability depends on each platform’s review; a prepared or submitted listing is not an approval.

No shared server token or Bruno password belongs in an MCP configuration file or conversation. Native OAuth discovers the server metadata and handles the authorization flow.

## First requests

Start with these read-only requests:

- Show my Bruno identity and accessible companies.
- List the agents in the selected company.
- Find and read a document I can access in that company.

Use the exact opaque `workspace_id` returned by `bruno_list_workspaces` in later commands. A numeric Bruno business-file ID is a different identifier.

Keep the request key and operation ID returned by every mutation. If a response is interrupted, consult `bruno_get_operation` using the original key before sending another request. An accepted or queued operation is not proof that the business action succeeded.

## Working with agents and native functions

The catalogue includes commands for listing templates and agents, agent creation and its progress, messages, conversation history, durable missions and versioned file reads. Connections to other services require their own authorization and configuration.

To discover additional native operations:

1. Select a workspace from `bruno_list_workspaces`.
2. Open a private work session with `bruno_open_native_session`.
3. Page through `bruno_list_native_tools` and use the returned input schemas.
4. Use `bruno_read_native_tool` for operations classified as read-only.
5. For a change, prepare the proposal, review its exact arguments and digest, then explicitly confirm the prepared action. Ordinary conversation text does not replace a structured approval.

The native catalogue is permission-dependent. An operation absent from the current catalogue is unavailable to that connection. Current company, user, agent and object permissions are checked again when an operation runs.

## OpenAI public profile

The restricted profile used by the OpenAI submission has a separate resource URL:

```text
https://bruno.incwo.com/api/platform/v1/mcp/openai
```

Use this profile with an existing Bruno account. It excludes account registration, credential entry, subscription offers, checkout and transactional billing-portal links. Its permissions and native-operation catalogue are restricted on the server.

The general profile includes additional account and subscription capabilities. Hosted payment links do not directly charge a card or activate an unpaid subscription; payment remains on the payment provider's page.

## Claude directory profile

The dedicated read-only profile prepared for Claude’s directory has a separate resource URL:

```text
https://bruno.incwo.com/api/platform/v1/mcp/claude
```

This profile is deployed and has been submitted to Claude’s directory; its status is In review. It exposes 15 consultation tools with nine read scopes: identity, companies, members, existing agents, MCP-registered conversations and work orders, existing agent-creation records, files and artifacts, billing overview and platform events. It does not execute AI agents, generate images, video or audio, modify data, invite colleagues, initiate payments or expose generic native dispatch. Member and billing information require company administrator rights. Existing sharing rules still apply.

The general profile retains its broader capabilities. The Claude profile uses a separate OAuth audience; its token cannot be used on the general or OpenAI endpoints. Its descriptions and public listing reflect this restricted scope.

For a custom Claude connection, add the dedicated URL and complete **Connect and authorize**. The full general endpoint remains available to clients and workflows that support its wider capabilities.

## OAuth and administrator notes

- Issuer: `https://bruno.incwo.com`.
- OAuth discovery: `https://bruno.incwo.com/.well-known/oauth-authorization-server`.
- Protected-resource metadata is advertised by the MCP server's anonymous HTTP 401 challenge.
- Native dynamic client registration, exact registered callback URLs, authorization codes and PKCE S256 are supported.
- Access tokens are signed, audience-bound and expire after ten minutes. Refresh credentials rotate and have a maximum absolute lifetime of thirty days.
- Native consent binds an existing signed-in Bruno account. It does not manufacture a universally verified email claim. Operations that separately require a verified email keep that requirement.
- Requested scopes are ceilings; current Bruno memberships, agent rights and local client permissions can further reduce access.

The documented flow uses individual Bruno login and consent. A separate enterprise managed-auth or client-credentials setup is not documented or claimed here.

To remove a connection, use the client's disconnect control and the Bruno client-revocation tool where applicable. `bruno_revoke_client` revokes that current MCP client; it does not close a company or cancel a paid subscription.

## Data and support

Bruno processes the account information, prompts, files and agent actions necessary for the functions you request. Consult the current service documents for retention, providers, user rights and applicable terms:

- [Privacy policy](https://get-bruno.com/confidentialite)
- [Service terms](https://get-bruno.com/conditions-generales)
- [Support](https://go.incwo.com/contact/)
- [Bruno product website](https://get-bruno.com)

Use synthetic data in a dedicated test workspace when evaluating a connector. Authorize external communications, purchases and other consequential changes explicitly.

## Registry and repository contents

The remote server metadata is published as [io.github.incwo/bruno 1.14.0 in the official MCP Registry](https://registry.modelcontextprotocol.io/v0.1/servers/io.github.incwo%2Fbruno/versions/1.14.0). Registry metadata, client directory review and software deployment are distinct states. The Claude directory submission is In review. OpenAI submission is being finalized. No directory approval is claimed by this repository.

This repository contains connection documentation and public server metadata. The Bruno service is operated by incwo; service use is subject to the terms linked above.
