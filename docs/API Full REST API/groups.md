# Group and Group Connection API

The Group and Group Connection API allows users to manage groups, join or leave them, and handle invitations and membership requests. It supports fetching group data, sending invites, processing join requests, and managing group members.

---

## Groups

### Fetch All Groups

### Scope

Retrieve the list of groups available on the platform. If the `with_full_data` flag is provided, full group details are included.

### Method

!!! info annotate "GET"
	```plaintext
	/api/v3/groups
	```

### Attributes

None

### Headers

| Name                  | Type   | Description |
|-----------------------|--------|-------------|
| `authentication-token` | string | Found in sign-in or sign-up responses. This token is used for user sessions |

### Responses

| HTTP status code | content-type    | Description                              |
|------------------|-----------------|------------------------------------------|
| `200`            | application/json| List of groups.                          |
| `500`            | text/plain      | Unknown server error.                    |

### Response object

| Attribute name | Type   | Description                   |
|----------------|--------|-------------------------------|
| `groups`       | array  | List of groups, with basic information. |

#### Group Object Structure

Each group contains:

| Attribute name    | Type    | Description                                    |
|-------------------|---------|------------------------------------------------|
| `id`              | integer | Group ID                                       |
| `name`            | string  | Name of the group                              |
| `group_type`      | string  | The type of group                              |
| `can_auto_join`   | boolean | Whether users can join the group automatically |
| `is_invite_only`  | boolean | Whether the group requires an invite to join   |
| `max_group_size`  | integer | The maximum number of members allowed in the group |
| `searchable`      | boolean | Whether the group is searchable by other users |
| `member_count`    | integer | Number of members in the group                 |

### Examples

!!! example
	#### cURL

	```shell
	curl --request GET \
		--header "authentication-token: abc123" \
		"https://gamefuse.co/api/v3/groups"
	```

	#### Response

	```json
	{
	  "groups": [
		{
		  "id": 1,
		  "name": "Awesome Gamers",
		  "group_type": "Public",
		  "can_auto_join": true,
		  "is_invite_only": false,
		  "max_group_size": 50,
		  "searchable": true,
		  "member_count": 12
		},
		{
		  "id": 2,
		  "name": "VIP Squad",
		  "group_type": "Private",
		  "can_auto_join": false,
		  "is_invite_only": true,
		  "max_group_size": 10,
		  "searchable": false,
		  "member_count": 5
		}
	  ]
	}
	```

---

### Fetch Group Details

### Scope

Retrieve the full details of a specific group, including its members, admins, join requests, and invites if available.

### Method

!!! info annotate "GET"
	```plaintext
	/api/v3/groups/{id}
	```

### Attributes

| Name  | Type    | Required | Description                        |
|-------|---------|----------|------------------------------------|
| `id`  | integer | Yes      | The ID of the group to fetch details for. |

### Headers

| Name                  | Type   | Description |
|-----------------------|--------|-------------|
| `authentication-token` | string | Found in sign-in or sign-up responses. This token is used for user sessions |

### Responses

| HTTP status code | content-type    | Description                              |
|------------------|-----------------|------------------------------------------|
| `200`            | application/json| Detailed information about the group.    |
| `500`            | text/plain      | Unknown server error.                    |

### Response object

| Attribute name    | Type    | Description                                    |
|-------------------|---------|------------------------------------------------|
| `id`              | integer | Group ID                                       |
| `name`            | string  | Name of the group                              |
| `group_type`      | string  | The type of group                              |
| `can_auto_join`   | boolean | Whether users can join the group automatically |
| `is_invite_only`  | boolean | Whether the group requires an invite to join   |
| `max_group_size`  | integer | The maximum number of members allowed in the group |
| `searchable`      | boolean | Whether the group is searchable by other users |
| `member_count`    | integer | Number of members in the group                 |
| `members`         | array   | List of members in the group (if `with_full_data` is true) |
| `admins`          | array   | List of admins in the group (if `with_full_data` is true) |
| `join_requests`   | array   | List of join requests for the group (if available) |
| `invites`         | array   | List of invites to the group (if available)    |

### Examples

!!! example
	#### cURL

	```shell
	curl --request GET \
		--header "authentication-token: abc123" \
		"https://gamefuse.co/api/v3/groups/1"
	```

	#### Response

	```json
	{
	  "id": 1,
	  "name": "Awesome Gamers",
	  "group_type": "Public",
	  "can_auto_join": true,
	  "is_invite_only": false,
	  "max_group_size": 50,
	  "searchable": true,
	  "member_count": 12,
	  "members": [
		{
		  "id": 10,
		  "username": "john_doe",
		  "email": "john.doe@example.com",
		  "display_email": "john.doe@example.com",
		  "credits": 150,
		  "score": 1200
		}
	  ],
	  "admins": [
		{
		  "id": 10,
		  "username": "john_doe",
		  "email": "john.doe@example.com",
		  "display_email": "john.doe@example.com",
		  "credits": 150,
		  "score": 1200
		}
	  ],
	  "join_requests": [],
	  "invites": []
	}
	```

---

## Group Connections

### Send Group Connection Request

### Scope

Create a new group connection, which may involve sending an invite or processing a membership request.

### Method

!!! info annotate "POST"
	```plaintext
	/api/v3/group_connections
	```

### Attributes

| Name      | Type    | Required | Description                             |
|-----------|---------|----------|-----------------------------------------|
| `group_id`| integer | Yes      | The ID of the group to connect to.      |
| `user_id` | integer | Yes      | The ID of the user requesting to join the group or being invited. |

### Headers

| Name                  | Type   | Description |
|-----------------------|--------|-------------|
| `authentication-token` | string | Found in sign-in or sign-up responses. This token is used for user sessions |
| `Content-Type`         | string | Set it to `application/json` |

### Responses

| HTTP status code | content-type    | Description                              |
|------------------|-----------------|------------------------------------------|
| `200`            | application/json| Group connection created successfully.   |
| `422`            | text/plain      | Invalid parameters or already connected. |
| `500`            | text/plain      | Unknown server error.                    |

### Response object

| Attribute name | Type   | Description                    |
|----------------|--------|--------------------------------|
| `id`           | integer| Group connection ID            |
| `status`       | string | The status of the group connection (`pending`, `accepted`, etc.) |
| `inviter_id`   | integer| ID of the user who sent the invite or created the connection |
| `user`         | object | The user involved in the group connection |

### Examples

!!! example
	#### cURL

	```shell
	curl --request POST \
		--header "authentication-token: abc123" \
		--header "Content-Type: application/json" \
		--data '{"group_id": 1, "user_id": 10}' \
		"https://gamefuse.co/api/v3/group_connections"
	```

	#### Response

	```json
	{
	  "id": 123,
	  "status": "pending",
	  "inviter_id": 1,
	  "user": {
		"id": 10,
		"username": "john_doe",
		"email": "john.doe@example.com",
		"display_email": "john.doe@example.com",
		"credits": 150,
		"score": 1200
	  }
	}
	```

---

### Manage Group Membership Requests

### Scope

Handle join requests and invites for a group by accepting or rejecting them.

### Method

!!! info annotate "PUT"
	```plaintext
	/api/v3/group_connections/{id}
	```

### Attributes

| Name       | Type    | Required | Description                  |
|------------|---------|----------|------------------------------|
| `id`       | integer | Yes      | The group connection ID.      |
| `status`   | string  | Yes      | New status for the group connection (`accepted`, `declined`). |

### Headers

| Name                  | Type   | Description |
|-----------------------|--------|-------------|
| `authentication-token` | string | Found in sign-in or sign-up responses. This token is used for user sessions |
| `Content-Type`         | string | Set it to `application/json` |

### Responses

| HTTP status code | content-type    | Description                              |
|------------------|-----------------|------------------------------------------|
| `200`            | application/json| Group connection updated successfully.   |
| `422`            | text/plain      | Invalid status or unauthorized action.   |
| `500`            | text/plain      | Unknown server error.                    |

### Response object

| Attribute name | Type   | Description                    |
|----------------|--------|--------------------------------|
| `id`           | integer| Group connection ID            |
| `status`       | string | The new status of the group connection (`accepted`, `declined`) |

### Examples

!!! example
	#### cURL

	```shell
	curl --request PUT \
		--header "authentication-token: abc123" \
		--header "Content-Type: application/json" \
		--data '{"status": "accepted"}' \
		"https://gamefuse.co/api/v3/group_connections/123"
	```

	#### Response

	```json
	{
	  "id": 123,
	  "status": "accepted"
	}
	```