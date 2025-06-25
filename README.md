#  Brick Game using Embedded Wizard on STM32F412G-DISCO

##  Overview
This project is a simple brick-breaking game built for the **STM32F412G-DISCO** board using **Embedded Wizard**. The player controls a paddle at the bottom of the screen. A ball bounces off the paddle and breaks bricks at the top. Each successful collision with a brick increases the score.

---

##  Hardware Used
- **STM32F412G-DISCO** development board
- ARM Cortex-M4 processor
- TFT LCD display (onboard)
- Touch screen interface (for paddle control)

---

##  Tools & Software
- **Embedded Wizard Studio** (for GUI development)
- **STM32CubeMX** (for board support configuration)
- **STM32CubeIDE** (for backend C code integration)
- **Embedded Wizard Platform Package for STM32F412G-DISCO**

---

##  Game Features
- Touch-based paddle movement  
- Ball physics (bouncing off paddle, walls, and bricks)  
- Brick destruction  
- Real-time score display  
- Game over condition when ball falls below paddle

---

##  Steps Followed in Embedded Wizard

### 1. **Project Setup**
- Selected the STM32F412G-DISCO platform package in Embedded Wizard.
- Created a new GUI project using the STM32 template.

### 2. **Creating UI Elements**
- **Paddle**: A movable rectangular widget at the bottom.
- **Ball**: A circular shape widget with motion logic.
- **Bricks**: Repeated widgets placed in a grid layout.
- **Score Display**: A text field showing the current score.

### 3. **Logic Binding**
- Attached an event handler to the touch screen to control the paddle's position.
- Implemented timers and update methods to animate the ball.
- Used hit-detection logic to check for collisions with paddle and bricks.

### 4. **Integration with STM32**
- Exported the GUI code as C code.
- Imported it into STM32CubeIDE.
- Compiled and flashed it to STM32F412G-DISCO using ST-LINK.

---

##  Game Logic Explanation

### Paddle Control
- The paddle moves horizontally based on the user's touch position.
- When the paddle is tapped or moved, the ball launches or changes direction.

### Ball Movement
- The ball continuously updates its `x` and `y` position using a timer-based tick method.
- The direction changes when it hits:
  - Side walls (reverse `x` velocity)
  - Top wall (reverse `y` velocity)
  - Paddle (reverse `y`, with slight angle changes based on paddle section)

### Brick Collision
- The ball checks collision with every active brick.
- On collision:
  - Brick becomes invisible or is removed
  - Ball `y` direction is reversed
  - Score is incremented

### Score Update
- A global variable `Score` is incremented and displayed on the screen.
- The text field is updated dynamically using the `UpdateViewState()` method.

---

##  Code Snippet (Simplified Ball Logic in Embedded Wizard)

```js
// This is pseudo JavaScript used in Embedded Wizard

function Timer_OnTrigger()
{
  ball.X += xSpeed;
  ball.Y += ySpeed;

  // Check wall collision
  if (ball.X <= 0 || ball.X >= ScreenWidth)
    xSpeed = -xSpeed;

  if (ball.Y <= 0)
    ySpeed = -ySpeed;

  // Paddle collision
  if (Touching(ball, paddle))
    ySpeed = -ySpeed;

  // Brick collision
  for (let brick of bricks)
  {
    if (brick.Visible && Touching(ball, brick)) {
      brick.Visible = false;
      ySpeed = -ySpeed;
      Score += 1;
      scoreText.String = "Score: " + Score;
    }
  }

  // Game over
  if (ball.Y > ScreenHeight)
    ShowGameOverScreen();
}
