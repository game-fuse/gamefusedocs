# Group and Group Connection API

The Group and Group Connection API allows users to manage groups, join or leave them, and handle invitations and membership requests. It supports fetching group data, sending invites, processing join requests, and managing group members.

---

## Create Group

### Scope

Allows a user to create a new group. Upon successful creation, the user will be made the group admin automatically.

### Method

!!! info annotate "POST"
	```plaintext
	/api/v3/groups
	```

### Attributes

| Name             				     | Type    | Required | Description                                       		 		      |
|------------------------------------|---------|----------|-----------------------------------------------------------------------|
| `name`                             | string  | Yes      | The name of the group                                  				  |
| `group_type`    		 		     | string  | No       | The type of the group (e.g., `Public`, `Private`, etc.) 			  |
| `max_group_size` 		 		     | integer | No       | The maximum number of members allowed in the group    				  |
| `can_auto_join`  		 		     | boolean | No       | Whether users can join the group automatically        				  |
| `is_invite_only` 		 		     | boolean | No       | Whether the group requires an invite to join          				  |
| `searchable`     		 		     | boolean | No       | Whether the group is searchable by other users     					  |
| `admins_only_can_create_attributes`| boolean | No       | Whether the admins are the only ones who can create attributes        |

### Headers

| Name                  | Type   | Description |
|-----------------------|--------|-------------|
| `authentication-token` | string | Found in sign-in or sign-up responses. This token is used for user sessions |
| `Content-Type`         | string | Set it to `application/json` |

### Responses

| HTTP status code | content-type    | Description                                      |
|------------------|-----------------|--------------------------------------------------|
| `201`            | application/json| Group created successfully.                      |
| `422`            | text/plain      | Could not create the group due to invalid data.  |
| `500`            | text/plain      | Unknown server error.                            |

### Response object

| Attribute name     | Type    | Description                                     |
|--------------------|---------|-------------------------------------------------|
| `id`               | integer | The ID of the created group                     |
| `name`             | string  | The name of the group                           |
| `group_type`       | string  | The type of the group (e.g., `Public`, `Private`)|
| `can_auto_join`    | boolean | Whether users can join the group automatically  |
| `is_invite_only`   | boolean | Whether the group requires an invite to join    |
| `max_group_size`   | integer | The maximum number of members allowed in the group |
| `searchable`       | boolean | Whether the group is searchable by other users  |
| `member_count`     | integer | The current number of members in the group      |
| `members`          | array   | List of members in the group (if `with_full_data` is true) |
| `admins`           | array   | List of admins in the group (if `with_full_data` is true) |
| `join_requests`    | array   | List of join requests (if available)            |
| `invites`          | array   | List of invites (if available)                  |

#### Member and Admin Object Structure

Each `member` and `admin` contains:

| Attribute name | Type    | Description                                     |
|----------------|---------|-------------------------------------------------|
| `id`           | integer | User ID                                         |
| `username`     | string  | Username of the user                            |
| `email`        | string  | Email of the user                               |
| `credits`      | integer | User credits                                    |
| `score`        | integer | User score                                      |

### Examples

!!! example
	#### cURL

	```shell
	curl --request POST \
		--header "authentication-token: abc123" \
		--header "Content-Type: application/json" \
		--data '{
			"name": "Awesome Gamers",
			"group_type": "Public",
			"max_group_size": 50,
			"can_auto_join": true,
			"is_invite_only": false
		}' \
		"https://gamefuse.co/api/v3/groups"
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
	  "member_count": 1,
	  "members": [
		{
		  "id": 10,
		  "username": "john_doe",
		  "email": "john.doe@example.com",
		  "credits": 150,
		  "score": 1200
		}
	  ],
	  "admins": [
		{
		  "id": 10,
		  "username": "john_doe",
		  "email": "john.doe@example.com",
		  "credits": 150,
		  "score": 1200
		}
	  ],
	  "join_requests": [],
	  "invites": []
	}
	```

---


## Fetch All Groups

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

## Fetch Group Details

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


## Send Group Connection Request

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

## Manage Group Membership Requests

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
# Group Attributes
	
## Fetch Group Attributes

### Scope

Retrieve all attributes for a specific group.

### Method
	
!!! info annotate "GET"
		```plaintext
		/api/v3/groups/{id}/attributes
		```
	
### Attributes
	
| Name  | Type    | Required | Description                        |
|-------|---------|----------|------------------------------------|
| `id`  | integer | Yes      | The ID of the group to fetch attributes for. |
	
### Headers
	
| Name                  | Type   | Description |
|-----------------------|--------|-------------|
| `authentication-token` | string | Found in sign-in or sign-up responses. This token is used for user sessions |

### Responses

| HTTP status code | content-type    | Description                              |
|------------------|-----------------|------------------------------------------|
| `200`            | application/json| Group attributes fetched successfully.   |
| `403`            | text/plain      | User does not have permission to view this group. |
| `500`            | text/plain      | Unknown server error.                    |

### Response object

| Attribute name            | Type    | Description                                   |
|---------------------------|---------|-----------------------------------------------|
| `id`                      | integer | The attribute ID                              |
| `key`                     | string  | The key for the attribute                     |
| `value`                   | string  | The value associated with the key             |
| `creator_id`              | integer | The ID of the user who created the attribute  |
| `can_edit`                | boolean | Whether the current user can edit the attribute |

### Examples

!!! example
	#### cURL

	```shell
		curl --request GET \
			--header "authentication-token: abc123" \
			"https://gamefuse.co/api/v3/groups/1/attributes"
	```
	
	#### Response
	
	```json
		{
		  "attributes": [
			{
			  "id": 1,
			  "key": "setting1",
			  "value": "decrypted_value1",
			  "creator_id": 1001,
			  "can_edit": true
			},
			{
			  "id": 2,
			  "key": "setting2",
			  "value": "decrypted_value2",
			  "creator_id": 1002,
			  "can_edit": false
			}
		  ]
		}
	```
	
	---
	
## Add Group Attributes

### Scope

Add or update attributes for a specific group. Only group admins or the attribute creator can update attributes, depending on group settings. All group members can read these settings.
If group.admins_only_can_create_attributes = true, non-admins in the group will NOT be able to create group attributes.
Attributes can be protected by the creator by passing in only_can_edit_by_creator = true, which will prevent others from modifying this attribute.

### Method

!!! info annotate "POST"
	```plaintext
	/api/v3/groups/{id}/add_attribute
	```

### Attributes

| Name        | Type    | Required | Description                                       |
|-------------|---------|----------|---------------------------------------------------|
| `id`        | integer | Yes      | The ID of the group to add attributes to.         |
| `attributes`| array   | Yes      | Array of attributes to be added/updated.          |

Each attribute object should have the following:

| Name                     | Type    | Required | Description                                        |
|--------------------------|---------|----------|----------------------------------------------------|
| `key`                     | string  | Yes      | The key for the attribute                          |
| `value`                   | string  | Yes      | The value associated with the key                  |
| `only_can_edit_by_creator`| boolean | No       | Whether only the creator can edit this attribute (defaults to `true`) |

### Headers

| Name                  | Type   | Description |
|-----------------------|--------|-------------|
| `authentication-token` | string | Found in sign-in or sign-up responses. This token is used for user sessions |
| `Content-Type`         | string | Set it to `application/json` |

### Responses

| HTTP status code | content-type    | Description                              |
|------------------|-----------------|------------------------------------------|
| `200`            | application/json| Group attributes added/updated successfully. |
| `403`            | text/plain      | User does not have permission to add attributes. |
| `400`            | text/plain      | Missing or invalid parameters.           |
| `500`            | text/plain      | Unknown server error.                    |

### Response object

| Attribute name            | Type    | Description                                   |
|---------------------------|---------|-----------------------------------------------|
| `id`                      | integer | The attribute ID                              |
| `key`                     | string  | The key for the attribute                     |
| `value`                   | string  | The value associated with the key             |
| `creator_id`              | integer | The ID of the user who created the attribute  |
| `can_edit`                | boolean | Whether the current user can edit the attribute |

### Examples

!!! example
	#### cURL

	```shell
	curl --request POST \
		--header "authentication-token: abc123" \
		--header "Content-Type: application/json" \
		--data '{
			"attributes": [
				{"key": "new_setting", "value": "new_value"},
				{"key": "another_setting", "value": "another_value"}
			]
		}' \
		"https://gamefuse.co/api/v3/groups/1/add_attribute"
	```

	#### Response

	```json
	{
	  "attributes": [
		{
		  "id": 3,
		  "key": "new_setting",
		  "value": "new_value",
		  "creator_id": 1001,
		  "can_edit": true
		},
		{
		  "id": 4,
		  "key": "another_setting",
		  "value": "another_value",
		  "creator_id": 1001,
		  "can_edit": true
		}
	  ]
	}
	```