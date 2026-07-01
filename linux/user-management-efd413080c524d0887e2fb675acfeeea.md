# User Management efd413080c524d0887e2fb675acfeeea

Here is a list that will help us to better understand and deal with user management.

| **Command** | **Description**                                                                                                                                            |
| ----------- | ---------------------------------------------------------------------------------------------------------------------------------------------------------- |
| `sudo`      | Execute command as a different user.                                                                                                                       |
| `su`        | The `su` utility requests appropriate user credentials via PAM and switches to that user ID (the default user is the superuser). A shell is then executed. |
| `useradd`   | Creates a new user or update default new user information.                                                                                                 |
| `userdel`   | Deletes a user account and related files.                                                                                                                  |
| `usermod`   | Modifies a user account.                                                                                                                                   |
| `addgroup`  | Adds a group to the system.                                                                                                                                |
| `delgroup`  | Removes a group from the system.                                                                                                                           |
| `passwd`    | Changes user password.                                                                                                                                     |

## Practical Juice

<details>

<summary>Which option needs to be set to create a home directory for a new user using "useradd" command?</summary>

```bash
-m
```

</details>

<details>

<summary>Which option needs to be set to lock a user account using the "usermod" command? (long version of the option)</summary>

```bash
—lock
```

</details>

<details>

<summary>Which option needs to be set to execute a command as a different user using the "su" command? (long version of the option)</summary>

```bash
—command
```

</details>
