Tetris Game
============

## Purpose

Simple exercise of creating an architecture of a computer game. Tetris game is an easy use case as it has well defined rules.

In this example the following process is followed: 
* **Define rules of the game** 
  
  Very important step. This is a definition of `WHAT` we want to achieve. What kind of product needs to be build.

* **Create requrements** 
  
  Also very important step. This is a list of architectural decisions that will define scope of work, limitations and restrictions. Any compromise that has to be made must be expressed here as a requrement. It is a contract between a customer and a provider. Each requirement has a tag and must be referenced in a design and later in source code. If the code does not reference any requirement, its existance should be questioned. This is also an input for validation to define test criteria and make sure the contract has been met.

* **Use Cases**

  First step to understand functional requirements. It helps to analyse the product and define interactions between system components. At this step non functional requirements may be captured and the list of requirements have to be updated. 

* **Sequence and Collaboration diagrams**

  Visualisation of what happens in the system. More non functional requirements may be discovered here.

## Table of Content

- [Tetris Game](#tetris-game)
  - [Purpose](#purpose)
  - [Table of Content](#table-of-content)
  - [Game Definition](#game-definition)
  - [Requirements](#requirements)
    - [Defined figures](#defined-figures)
  - [Use Cases](#use-cases)
    - [State Control](#state-control)
    - [Time Progress](#time-progress)
    - [Player Generates Input](#player-generates-input)
  - [Class Diagrams](#class-diagrams)
    - [Overal Colaboration](#overal-colaboration)
    - [Game Environment](#game-environment)
    - [Commands](#commands)
    - [Game Figures](#game-figures)
  - [Sequence Diagrams](#sequence-diagrams)
    - [Start Program](#start-program)
    - [Restart Game](#restart-game)
    - [Successful Figure Rotation](#successful-figure-rotation)
    - [Failed Figure Rotation](#failed-figure-rotation)
    - [Successful Figure Translation](#successful-figure-translation)
    - [Failed Figure Translation](#failed-figure-translation)
    - [Move Down On Timer Tick Event](#move-down-on-timer-tick-event)
    - [Figure Life Time](#figure-life-time)
    - [Checking Figure Overlap](#checking-figure-overlap)
    - [Dropping Figure](#dropping-figure)
    - [Game Over](#game-over)


## Game Definition

I want to write a game where different figures fall from the top of the screen to the bottom. One at the time. Player can shift the figures left and right and also can rotate the figure in both directions. If the player does nothing, the figure will advance one step down after some time. Once the figure is at the bottom, it cannot move anymore. Figure stays there. A new figure shows up at the top of the screen. By moving figures, player can control where the figures will be placed at the botom of the screen. Figures are made of blocks. Screen is made of lines, which are made of blocks as well. Once figures fill in a full line with blocks, that line is removed from the screen. Any remaining blocks above that line will drop one line down.

Game last as long as player can make lines to be removed so there is a space for new figures.

Blocks cannot overlap. So, the game is over when it is impossible to place a new figure on the top of the screen.


## Requirements 

| Requirement | Description |
|-----|-------------|
| <a id="REQ_SinglePlayer">REQ_SinglePlayer</a> | There is a single player |
| <a id="REQ_Cmd">REQ_Cmd</a>| Player can enter single control command at one time. Commands are: Translate Left, Translate Right, Translate Down, Rotate Left, Rotate Right.
| <a id="REQ_LineOfBlocks">REQ_LineOfBlocks</a> | Single line is made of blocks.
| <a id="REQ_LineFull">REQ_LineFull</a> | Line that has no empty spaces, that is, when is made of blocks, is removed from the screen.
| <a id="REQ_ScreenSize">REQ_ScreenSize</a> | Screen has fixed dimentions of 10 rows and 8 columns. Screen is made of lines.
| <a id="REQ_BlocksDrop">REQ_BlocksDrop</a> | When line is removed from the screen, all the lines being above the removed line, are moved down. The most top line of the screen becomes empty. 
| <a id="REQ_FiguresType">REQ_FiguresType</a> | There is 5 [defined figures](#defined-figures) in the game
| <a id="REQ_MoveLimit">REQ_MoveLimit</a> | Figure must not be placed outside borders of the screen. That is, figure cannot move left when is at the left border. Figure cannot move right when is at the right border. Figure cannot move down when is at the bottom of the screen.
| <a id="REQ_FigureLifeTime">REQ_FigureLifeTime</a> | Player looses a control of a figure that cannot advance down (is at the bottom of the screen). New figure is generated at the top of the screen.
| <a id="REQ_BlocksNotOverlap">REQ_BlocksNotOverlap</a> | Single block takes space. Figures are made of blocks. Two blocks cannot take the same space. This implicates that figures cannot overlap. This also implicates that the figures cannot move (translate or rotate) when the new position would overlap with any block of a line.
| <a id="REQ_NoPendingCommands">REQ_NoPendingCommands</a> | Commands are not buffered. New command overwrites the previews one if that one has not be executed yet
| <a id="REQ_OnTimerCommand">REQ_OnTimerCommand</a> | Translate Down command will be generated after 1 second if player does not input any command in that period. 


### Defined figures 
* Square
  ```
    O
  ```

* BigSquare
  ```
    OO
    OO
  ```
* Bar
  ```
    O
    O
    O
  ```

* BarT
  ```
   OOO
    O
  ```

## Use Cases
### State Control

Player and timer trigger state changes. All state control happens in the main loop.

![State Control](http://www.plantuml.com/plantuml/png/5Son3G8n30NGdYbWWRYdEdQWHy5d9HOv9tA-XTYUI_MwjqraHnpjSbFZ5hjSAp3cdSZpDnL5ZNTCSUu6CIJk1nN_bUxoeQV0TJwSAwafQBHiAwDpXRmOtj9O-IQsd_u1)

### Time Progress

Timer event moves a figure only down. No rotation or shifting left or right.

![Time Progress](http://www.plantuml.com/plantuml/png/5Son3G8n30NGdYbWWRYdEdQWHo5d9HQ94zdVGcpFNVMwjpco8KQ_t4HBZvsl3LX-9xByJLNGuXtELgQ25QdCGTL-THf_wCamyOULHg82MZthcH5ay6lPhCRkQPt_nheV_W40)

### Player Generates Input

![Player Generates Input](http://www.plantuml.com/plantuml/png/5Oqn3iCW34NtdgAz0DuxfNVe7b5YWLLYaFbMnUqRflUczxQ1QF2ptXwYgVh1zmBqyIIo-0jPKFpZWoqr1Ij2QYTbcxaPV-dDC1alIuL41THhr1LRCjdspBgBTaVeQx6n7XV_)



## Class Diagrams
### Overal Colaboration

![Colaboration Diagram](http://www.plantuml.com/plantuml/png/5Ssn3G8n34RXdYbWWNDFTUn0Zto9bLWaTkJy4M9xkEfrxzidEB1wkzpKKbjPhXKOyquK_DcAegvZJOda1Z5ioJkL-1OFUTDJ43eVaIgfAMWqjdD6oHiV7WnrUsNb4jotSJAxMFxt1m00)

### Game Environment

![Game Environment](http://www.plantuml.com/plantuml/png/5Smn3W8X40NGtbFe1PZUQhs3lK7s1oR2G8QFnjlhLgzxssHqhbPF5xLKruvp8SUyYdZyXAmiwxacF7KZZ27r9jc_kJjUSWTTvyrJ4JMbZ1hjo2J7XWVi7SPHRx3zxmy0)


### Commands

![Commands](http://www.plantuml.com/plantuml/png/5Sn13W8X34RXlQVG2z2zg_4ENW2XWKOefFs9yVOphDxcvJiSqR1wkpr4KrkPxnMO_YIo-0j5KDTdELiQ2bQ4D3eL-vTX_AKtmyHVbYg92cYsg1kZw-fHiuUvwOCrNpq1)



### Game Figures

![Game Figures](http://www.plantuml.com/plantuml/png/5Sp14G8X30NGkrLe0-JkQUt11f2Da94DJF8pZjsxdDxjROviYADVRgAbnyxN1ao_4rd-fYfeyGfdAqF1YbJ6GTL-THX_wCamyOULHg82cZthokWjDOmzpJLlhFlf7m00)



## Sequence Diagrams

### Start Program

Creating a game means to create screen, generate a random figure and start the timer.

![Start Seqence Diagram](http://www.plantuml.com/plantuml/png/7Smn3i8m30NGdLF01UATgTo17ONKIYnI9zZVGjmUskbjRt5aGspzT14jFLO-Ds3wbSZo9rL1YsSuMnaALgGoDghshSFuHNU6YT-iD18LqEnPzuoStuzZJdE_5zRrxmy0)



### Restart Game

Restarting a game is to clear the screen and create a figure.

![Restart Sequence Diagram](http://www.plantuml.com/plantuml/png/7Smn3i8m30NGdLF01UATgTo17ONKIYnI9zZVGjmUskbjRt5aGspzT14jFLO-Ds3wbSZo9rL1YsSuMnaALgGoDghshSFuHNU6YT-iD18LqEnPzuoStm_ZuqvptnVMzUyF)



### Successful Figure Rotation

Figure rotation succeeds when the figure does not overlap any blocks after a rotation. Any new command arriving during that process is dropped.

![Figure Rotation Success](http://www.plantuml.com/plantuml/png/5Ssn4S8m34RXdYbWWQYd2ZlG-umYR1sERAJVnx4zf7hT-zP0TiozdIvgtEOcvmgCk19v_Yn5KUTZXZXtWnWMxZiL_vRkygAxm3LkpfNK53HQjcD68U_qr9Ay8yLabeXb7Cegjltx0m00)


### Failed Figure Rotation

Figure rotation fails when the figure overlaps other blocks after a rotation. Any new command arriving during that process is dropped.

![Figure Rotation Failed](http://www.plantuml.com/plantuml/png/5SqniW8X38Vn_ftYUG7IMwrti6SGif2H3PZy6SVRwrPVz_qsHpAie_zTH7DVXVyRCFrAPEaTgg2jntAsF1Ii27aSLErJX_6JxWmJlrbk92gWsPEUCt9-nLrgj86u9-bSmZoHZRNy-WK0)


### Successful Figure Translation

Figure translation succeeds when the figure does not overlap any blocks after a translation. Any new command arriving during that process is dropped.

![Figure Translation Success](http://www.plantuml.com/plantuml/png/5Ssn4S8m34RXdYbWWQYd2ZlG-umYR1sERAJVnx4zf7hT-zP0TiozdIvgtEOcvmgCk19v_Yn5KUTZXZXtWnWMxZiL_vRkygAxm3LkpfNK53HQjcD68U_qr9BWsQ9bI8h9B17BEAnLRFlt1m00)




### Failed Figure Translation

Figure translation fails when the figure overlaps other blocks after a translation. Any new command arriving during that process is dropped.

![Figure Translation Failure](http://www.plantuml.com/plantuml/png/5Sqnai8m34RXVa-nN23ggS8Tw3t2KMmDZcoatyDmUqZrklUj0NRCe_rTr7ARc_nNOCILoFcz54MTZndYkHl4iEH-KF5FwIvFkWjSu-Qvafg2HcCxPnJoIhkM16UBnf2qixPbdD0gjltw1G00)


### Move Down On Timer Tick Event

Time advances the figure down after a timeout. It is a regular translation process. 

![On Timer Move Down](http://www.plantuml.com/plantuml/png/5Oqn4a8X30LxJw4N-EcjldUmPv2VWOmGC7deU7kbscftTqSqp5PF5z5Kvy7d0Wo_4bdyR5bGvcCwM-eALcofDyhspTtuINU6YTyi5H8LqAXHLuoSNw5hW3IsmEybZZNPMOy_Vm00)




### Figure Life Time

A new figure is created and will live until it cannot move down anymore. If it can move, either user can request the figure to move or timer tick can move it down.

![Figure Life Time](http://www.plantuml.com/plantuml/png/5SqniW8X38Vn_ftYUG7IMwrti6UGpK9629Zy6SVRwrPVz_qsHpAitlukehcjdVyhCFrAPEaTYg2kntAsF1Ii27bqAlOfm_X9TuQ9tonN4XNGR4dF6Jc_uY4bDZauGZg7UHeRw_xh5m00)



### Checking Figure Overlap

A figure cannot move beyond the screen's dimentions and it cannot overlap with blocks already on the screen.

![Checking Figure Overlap](http://www.plantuml.com/plantuml/png/5Sqn4W8X34RXtbFe1KXljTx1VWVP21dPWCcFZhTtgLxtliKH3MElfujegcFZSm6wNuaiVaCMbF4OpfQQWXKXjPso_N8pVj6TOU8tIuL41THhr3aZv_VwbBos3rkDFVGfBApRqJy0)



### Dropping Figure

This is a use case when a figure cannot move down anymore. Its blocks are taken to fill empty gaps. In a case when a line does not have gaps (line is full), the line will be removed and all lines above are moved down. It is recursive operation from the most bottom line to the most top.

![Drop Figure](http://www.plantuml.com/plantuml/png/5Smn4W8X30NGtbFe1KXljTx1VWSXGaOsOF8PnzihrUlUveY6URJJHJIrcVbSWU5N8ekVK0HrFgQpfQvWHKZze6BVqerVT6UO-CrI9L41ZHPrPUJytfzIzcnzXB7wZJN__G40)




### Game Over

Game is over when a new figure cannot be created. This will be a case when checking overlap sequence returns error.



![Game Over](http://www.plantuml.com/plantuml/png/5Smn4W8X30NGtbFe1KXljTx1VWVP36I4WCb7nzihrUlUveY6sUhf8fhgFFXSWE5N8ekVa0LbFgQpfQvWHKYzKRPlxiPFkZDC_6QfaAY0HeswCd9-xJastlkRBOprRwp7tny0)

