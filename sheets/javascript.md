##Closure example##

```js
function ticketmaker(transport) {
  return function(name) {
    alert("Heres a ticket" + name + "for the" + transport)
  }
}

issueBusTicket = ticketmaker(Bus)

issueTrainTicket = ticketmaker(Train)

issuePlaneTicket = ticketmaker(Plane)

issueBusTicket("Rob")
```