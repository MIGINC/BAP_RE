# Opcodes and Function Classes

The opcode is only 3 bits, and what it means depends on the **class** of the function it's aimed at. Every function has exactly one class, fixed in the function catalogue for that LSG.

## The four classes

- **Property**: a single value, or a fixed record. Most functions are properties: a setting, a status, a set of tyre pressures.
- **Array**: a list the ASG pages through. Phonebook entries, call lists, the lines of a cluster menu. See [Arrays](arrays.md).
- **Method**: an action that takes time. Dial a number, start a download. The FSG can report *Processing* while it works, then a *Result*.
- **Cache**: only one of these exists, **GetAll on FCT 0x01**. It's the "give me everything" request used at start-up.

I had an "Event" class in my own code for a while. It doesn't exist.

## Opcode table

| Op | Property | Array | Method | Cache | Sent by |
|---|---|---|---|---|---|
| **0** | Set · Reset¹ | SetArray | Start | – | ASG (Reset: FSG) |
| **1** | Get | GetArray | Abort | GetAll | ASG |
| **2** | SetGet | SetGetArray | StartResult | – | ASG |
| **3** | HeartbeatStatus | ChangedArray | Processing | – | FSG |
| **4** | Status | StatusArray | Result | StatusAll | FSG |
| **5** | StatusAck | – | – | – | FSG |
| **6** | Ack | – | – | – | ASG |
| **7** | Error | Error | Error | Error | FSG |

¹ Reset shares opcode 0 with Set, and in practice it only ever appears on **BAP_Config**. It's how an FSG announces "I've just started, re-sync with me". See [Session Lifecycle](session-lifecycle.md).

A handy rule of thumb: **0–2 are requests from the ASG, 3–5 come from the FSG, 6 is the ASG's reply to a 5, and 7 is the FSG saying no.**

### StatusAck and Ack

Most functions never use 5 and 6, and a plain Status is never acknowledged. But they are real. The parking module does it on LSG 0x0A FCT 0x15 (logged in car):

```plaintext
0x6DA  52 95 11 03     FSG: StatusAck (op 5), LSG 0x0A, FCT 0x15, 11 03
0x67A  62 95 11 03     ASG: Ack (op 6), same payload, 14 to 42 ms later
```

The Ack echoes the payload byte for byte, in all 93 pairs in my car logs. If you're emulating the ASG side of parking, send it. Only a few functions work this way, so don't Ack an ordinary Status.

eCall (0x33) 0x10 and 0x11 are two more (simulated, application layer), see [MIB2 Results](mib2.md#statusack).

## What the opcodes look like in practice

**Get / SetGet → Status.** The cluster reads with Get and writes with SetGet. The FSG answers both with Status and the value it now holds. If the request was out of range or refused, that isn't the value that was asked for. From a real car, Side Assist on/off:

```plaintext
cluster → 0x676  26 93 01        SetGet, LSG 0x1A, FCT 0x13, value 01
```

**Set** is a write that doesn't ask for anything back. The cluster uses it for small one-way signals, like acknowledging a Secondary Display frame.

**HeartbeatStatus vs Status.** The payload is the same. Status answers a request or reports a change. HeartbeatStatus is the FSG's periodic "still here, still this value". An ASG can treat both as the current value.

**GetAll → StatusAll.** One request, one big answer holding every cached property. Real example from the cluster to the RDK:

```plaintext
cluster → 0x6C8  11 C1           GetAll (op 1), LSG 0x07, FCT 0x01, no payload
RDK     → 0x6DD  80 27 41 C1 …   StatusAll (op 4), 39 bytes, segmented
```

**Methods** are for things that don't finish straight away. The ASG sends Start (or StartResult if it wants a Result back). The FSG replies Processing to keep the ASG waiting, then Result. Abort cancels. Phone functions like DialNumber, HangupCall and AcceptCall are methods.

**Error** is a single byte of error code from the FSG, in answer to something it couldn't do. See [Errors](errors.md).

## Why the class matters when you implement it

The class decides which opcodes are legal, in which direction, and the payload layout. An array's Status carries an array header, a property's doesn't. Set it wrong and you'll reject valid opcodes.

Related: [Frames on the Wire](frames.md) · [Generic Functions](generic-functions.md) · [Arrays](arrays.md)
