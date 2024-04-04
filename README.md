# Cubic Creature Simulation

A Creative Project For Fun, By GreatCat42

---

## Brief introduction

**Creatures** **move around** the **3D Canvas**, building **Connections** and transferring **Impulses** by sending **Letters** that contain **Messages**.

## Creature

A *creature* appears as a green, reflective cube. This simuation is centered on the behavior of creatures.

## States of Movement

A creature's *state of movement* is **the manner in which it is moving**. The following list shows every possible state of movement:

```
// Entering these states consumes 1 mobility point
* jumping
* rolling
* sliding forward

// Entering these states does not consume mobility points
* stationary
* switching direction
* turning
```

When a creature's non-stationary state ends, it will enter *stationary*. 

## Mobility Points

A Creature's *mobility points* **determines the likelihood it is to enter a non-stationary state from stationary**. The higher it is, the more likely. The following list describes this relationship:

```
points / likelihood(chance per tick)
------------------------------------
0 / 0
1 / 0.002
2 / 0.01
3 / 0.05
4 / 0.1
5 / 1
```

A creature has a capacity for *at most* 5 mobility points. (As will be mentioned below, an exception restricts it down to 2)

## The 3D Canvas

*The 3D Canvas* **limits the creatures in a specific, rectangular region in front of the camera**, using a mechanism similar to the game *Snakes*.

## Connection

A *connection* is a **one-way relationship between two creatures**, and **creatures use them to find targets to fire impulses at**. A *primary* connection is **the first connection that a creature has made**. A non-primary connection is forgotten by its owner creature after a certain time. The following graph shows how connections work in firing impulses.

```
A => B // Primary connection
A -> C // Non-primary connection

When A receives an impulse, it will look at its connections to other creatures:
| A => B
| A -> C
| A -> D
| A -> E

Then it generates a list of probability:
// 'W' stands for 'Weight'.
// The primary connection counts as 3 weight points, while each of the other connections counts as 1.
W(B) = 3;
W(C) = 1;
W(D) = 1;
W(E) = 1;
SW = 3+1+1+1 = 6;

P(to fire at X) = W(X)/SW
```

Creatures without primary connections have their mobility points maximum reduced to 2 points.

## Letter

**Creatures send messages** via *letters*. A letter does not pass its message to its sender. Once a letter has passed its message to a creature or fallen to the ground, it becomes ineffective and disappears from the screen.

## Message

Creatures use messages **to transfer impulses and build connections**. The possible content of a message is shown in the list below.

```
0 Impulse
1 Ask for a connection, from A, to B
2 Confirming a connection, from A, to B
```

## Inbox

The creature's *inbox* **stores messages for it to handle**, since creatures receive messages all the time but only handles them while being stationary. The inbox empties  *without any of its content being processed* when its owner creature's mobility points reaches 5.

## Impulse

The *impulse* is a **non-targeted message that pass from one creature to the next** through the chain-like structure formed by connections. Receiving an impulse adds 1 to the creature's mobility points. The impulse is then handled by firing a new impulse at a connected creature. Failure of handling (e.g. because the target is too far apart) does not trigger anything. A creature with 0 mobility points spontaneously fires impulses.

## Forming a connection

The *ask* and *confirm* are **messages used by creatures to establish new connections**. The *ask* is **non-targeted** while the *confirm* is **targeted at its `from` property**. Their action is demonstrated in the list below:

```
1 A creature (A) fires a letter containing an ASK message
 [A] --> BY_A *ASK from_A*

2 There is a possibility that the letter gets received by another creature (B)
 BY_A *ASK from_A* --> [B]

3 On receiving the ASK message, (B) fires back a CONFIRM message at (A).
 [B] --> BY_B *CONFIRM from_A to_B*

4 (A) receives the message from the letter
 BY_B *CONFIRM from_A to_B* --> [A]

5 (A) establishes a connection to (B).
 A -> B (or A => B)
```

A mis-targeted *confirm* message will be removed from the inbox without being handled.

 ---

Creative project

By GreatCat42



Planning: Sugar, notes, maths, flavours, happiness points, hibernation (Guess what they will be like)


