# API documentation

Public API Endpoint: https://api.trashgr.am/

**If you don't want to setup own Bot API Server, just deploy your bot on our API server :D**

Thanks to our server sponsor [@licson](https://github.com/licson).

---

If you want to use this API without error, you need to modify your Bot API library source code.

---

# Contents #

1. Original API Changes
   1. [Chat Permissions](#chat-permissions)
   2. [Chat Member](#chat-member)
   3. [User](#user)
   4. [File](#file)
   5. [Update Type](#update-type)
        1. [deleted_messages](#deleted_messages)
2. [Get message](#get-message)
3. [Get chat members](#get-chat-members)

---

## Chat Permissions ##

As we know, Official Bot API have 14 permissions field.

But in this version, we have 17 permissions field.

Cause `can_send_other_messages` combine several permissions.

1. `can_send_stickers`
2. `can_send_animations`
3. `can_send_games`
4. `can_use_inline_bots`

So Husky Bot API exposes `can_send_other_messages` and allow you restrict a group or a member to send stickers, animations, games or inline messages.

**Affected API**: [setChatPermissions](https://core.telegram.org/bots/api#setchatpermissions), [restrictChatMember](https://core.telegram.org/bots/api#restrictchatmember)

*New Chat Permissions List*

| Parameter                   | Type    | Description                                                                                                              |
|-----------------------------|---------|--------------------------------------------------------------------------------------------------------------------------|
| `can_send_basic_messages`   | Boolean | Optional. True, if the user can send text messages, contacts, invoices, locations, and venues                            |
| `can_send_audios`           | Boolean | Optional. True, if the user can send music files                                                                         |
| `can_send_documents`        | Boolean | Optional. True, if the user can send documents                                                                           |
| `can_send_photos`           | Boolean | Optional. True, if the user can send audio photos                                                                        |
| `can_send_videos`           | Boolean | Optional. True, if the user can send audio videos                                                                        |
| `can_send_video_notes`      | Boolean | Optional. True, if the user can send video notes                                                                         |
| `can_send_voice_notes`      | Boolean | Optional. True, if the user can send voice notes                                                                         |
| `can_send_polls`            | Boolean | Optional. True, if the user is allowed to send polls                                                                     |
| `can_send_stickers`         | Boolean | Optional. True, if the user is allowed to send stickers                                                                  |
| `can_send_animations`       | Boolean | Optional. True, if the user is allowed to send animations                                                                |
| `can_send_games`            | Boolean | Optional. True, if the user is allowed to send games                                                                     |
| `can_use_inline_bots`       | Boolean | Optional. True, if the user is allowed to use inline bots                                                                |
| `can_add_web_page_previews` | Boolean | Optional. True, if the user is allowed to add web page previews to their messages                                        |
| `can_change_info`           | Boolean | Optional. True, if the user is allowed to change the chat title, photo and other settings. Ignored in public supergroups |
| `can_invite_users`          | Boolean | Optional. True, if the user is allowed to invite new users to the chat                                                   |
| `can_pin_messages`          | Boolean | Optional. True, if the user is allowed to pin messages. Ignored in public supergroups                                    |
| `can_manage_topics`         | Boolean | Optional. True, if the user can manage topics                                                                            |
| **Compatibility**           |         |                                                                                                                          |
| `can_send_other_messages`   | Boolean | Optional. If you don't need to restrict `stickers`, `animations`, `games` and `inline bots`.                             |
| `can_send_media_messages`   | Boolean | Optional. If you don't need to restrict `audios`, `documents`, `photos`, `videos`, `video notes` and `voice notes`.      |

## Chat Member ##

`ChatMember` now has 2 new field.

| Property    | Type    | Description                                                  |
| ----------- | ------- | ------------------------------------------------------------ |
| `join_date` | Integer | Point in time (Unix timestamp) when the user joined the chat |
| `invite_by` | User    | A user that invited/promoted/banned this member in the chat  |

### User ###

`User` now has 3 new field.

| Property      | Type    | Description                             |
| ------------- | ------- | --------------------------------------- |
| `is_deleted`  | Boolean | True, if this user account is deleted.  |
| `is_verified` | Boolean | True, if this user account is verified. |
| `is_scam`     | Boolean | True, if this user account is scam.     |

### File ###

`File` now has 1 new field.

| Property     | Type    | Description                           |
| ------------ | ------- | ------------------------------------- |
| `file_dc_id` | Integer | File data center ID                   |
|              |         | ID => location                        |
|              |         | 1, 3 => Miami, Florida, United States |
|              |         | 2, 4 => Amsterdam, Netherlands        |
|              |         | 5 => Singapore                        |

### Update Type ###

Update Type now has 1 new type.

### `deleted_messages` ###

| Property      | Type             | Description |
| ------------- | ---------------- | ----------- |
| `chat`        | Chat             | Chat info   |
| `message_ids` | Array of Integer | Messages ID |

---

## Get message ##

Name: `getMessage`

Allow bot to get old message.

**Parameters:**

| Parameter    | Type              | Required? | Description                                                                                              |
| ------------ | ----------------- | --------- | -------------------------------------------------------------------------------------------------------- |
| `chat_id`    | Integer or String | Yes       | Unique identifier for the target chat or username of the target channel (in the format @channelusername) |
| `message_id` | Integer           | Yes       | Identifier of the message to get                                                                         |

**Returns:**

| Code | Scenarios           |
| ---- | ------------------- |
| 400  | message not found   |
| 200  | All other scenarios |

The response is a `message` type.

## Get chat members ##

Name: `getChatMembers`

Allow bot to get chat members list.

Field `offset`, `limit` and `type` only work for supergroup.

_Note: only 10000 supergroup and 200 channel members can be received through getChatMembers. The limit is server-side._.

**Parameters:**

| Parameter | Type              | Required? | Description                                                                                              |
| --------- | ----------------- | --------- | -------------------------------------------------------------------------------------------------------- |
| `chat_id` | Integer or String | Yes       | Unique identifier for the target chat or username of the target channel (in the format @channelusername) |
| `offset`  | Integer           | Optional  | Number of users to skip.                                                                                 |
| `limit`   | Integer           | Optional  | The maximum number of users be returned; up to 200.                                                      |
| `type`    | Integer           | Optional  | The type of users to return. By default, show all recent users without filter.                           |
|           |                   |           | 1=Administrators                                                                                         |
|           |                   |           | 2=Banned                                                                                                 |
|           |                   |           | 3=Restricted                                                                                             |
|           |                   |           | 4=Bots                                                                                                   |

**Returns:**

| Code | Scenarios                                       |
| ---- | ----------------------------------------------- |
| 400  | 1. The provided identifier is invalid           |
|      | 2. You haven't joined this channel/supergroup   |
|      | 3. You must be an admin in this chat to do this |
| 200  | All other scenarios                             |

The response is a `ChatMember` array.

