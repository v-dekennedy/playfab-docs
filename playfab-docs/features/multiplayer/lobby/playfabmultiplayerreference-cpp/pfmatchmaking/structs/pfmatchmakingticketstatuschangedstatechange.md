---
author: tomcoMSFT
title: "PFMatchmakingTicketStatusChangedStateChange"
description: "Information specific to the *TicketStatusChanged* type of state change."
ms.author: tomco
ms.topic: reference
ms.prod: playfab
ms.date: 11/09/2021
---

# PFMatchmakingTicketStatusChangedStateChange  

Information specific to the *TicketStatusChanged* type of state change.  

## Syntax  
  
```cpp
typedef struct PFMatchmakingTicketStatusChangedStateChange {  
    _Notnull_ ticket;  
} PFMatchmakingTicketStatusChangedStateChange  
```
  
### Members  
  
**`ticket`** &nbsp; _Notnull_  
  
The matchmaking ticket whose status changed.
  
The new ticket status can be retrieved via [PFMatchmakingTicketGetStatus](../functions/pfmatchmakingticketgetstatus.md).
  
  
## Requirements  
  
**Header:** PFMatchmaking.h
  
## See also  
[PFMatchmaking members](../pfmatchmaking_members.md)  

  
  
