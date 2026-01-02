# 🏓 PingPong Calculator

[![Live Demo](https://img.shields.io/badge/Demo-Live-brightgreen?style=for-the-badge)](https://pingpong-calculator-a77e8g6di-sajidmahamud835s-projects.vercel.app)
[![HTML5](https://img.shields.io/badge/HTML5-E34F26?style=flat&logo=html5&logoColor=white)](https://developer.mozilla.org/en-US/docs/Web/HTML)
[![Tailwind CSS](https://img.shields.io/badge/Tailwind_CSS-38B2AC?style=flat&logo=tailwind-css&logoColor=white)](https://tailwindcss.com/)
[![JavaScript](https://img.shields.io/badge/JavaScript-F7DF1E?style=flat&logo=javascript&logoColor=black)](https://developer.mozilla.org/en-US/docs/Web/JavaScript)

A sleek, modern calculator with satisfying ping-pong sound effects on every button press.

**🔗 [Live Demo](https://pingpong-calculator-a77e8g6di-sajidmahamud835s-projects.vercel.app)**

## ✨ Features

- **Glassmorphism Design** - Modern frosted glass aesthetic
- **Sound Effects** - Satisfying beep on every interaction
- **Keyboard Support** - Full keyboard navigation
- **Responsive** - Works on all screen sizes
- **No Build Required** - Pure HTML with Tailwind CDN

## 🚀 Usage

Simply open `index.html` in any modern browser.

### Keyboard Shortcuts

| Key | Action |
|-----|--------|
| `0-9` | Input numbers |
| `.` | Decimal point |
| `+ - * /` | Operations |
| `Enter` or `=` | Calculate |
| `Backspace` | Delete last digit |
| `Escape` or `Delete` | Clear all |

---

## 📚 Tutorial: Build It Step-by-Step

### Step 1: Create the HTML Structure

Create an `index.html` file with the basic structure:

```html
<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>Sleek Calculator</title>
    <script src="https://cdn.tailwindcss.com"></script>
</head>
<body>
    <!-- Calculator will go here -->
</body>
</html>
```

### Step 2: Configure Tailwind with Custom Colors

Add the Tailwind configuration for our dark theme colors:

```html
<script>
    tailwind.config = {
        theme: {
            extend: {
                colors: {
                    'dark-bg-start': '#1A1A2E',
                    'dark-bg-end': '#16213E',
                    'calc-body': 'rgba(25, 25, 40, 0.7)',
                    'btn-default': 'rgba(255, 255, 255, 0.08)',
                    'btn-hover': 'rgba(255, 255, 255, 0.15)',
                    'accent-teal': '#00BCD4',
                    'display-bg': 'rgba(0, 0, 0, 0.8)',
                    'display-curr': '#ECEFF1',
                }
            }
        }
    }
</script>
```

### Step 3: Add Glassmorphism CSS

Add custom styles for the glassmorphism effect:

```css
<style>
    .calculator-grid {
        backdrop-filter: blur(16px);
        -webkit-backdrop-filter: blur(16px);
        border: 1px solid rgba(255, 255, 255, 0.1);
    }
    
    .glow-on-hover:hover {
        box-shadow: 0 0 15px rgba(0, 188, 212, 0.6);
    }
</style>
```

### Step 4: Build the Calculator Layout

Create a 4-column grid for the calculator buttons:

```html
<body class="bg-gradient-to-br from-dark-bg-start to-dark-bg-end min-h-screen flex justify-center items-center">
    <div class="calculator-grid w-full max-w-sm rounded-3xl p-4 grid grid-cols-4 gap-3">
        <!-- Display -->
        <div class="col-span-4 bg-display-bg rounded-2xl p-5 text-right">
            <div data-previous-operand class="text-gray-400 text-xl"></div>
            <div data-current-operand class="text-white text-4xl font-bold">0</div>
        </div>
        
        <!-- Buttons -->
        <button data-all-clear class="col-span-2 bg-btn-default p-4 rounded-xl">AC</button>
        <button data-delete class="bg-btn-default p-4 rounded-xl">DEL</button>
        <button data-operator class="bg-btn-default p-4 rounded-xl">/</button>
        <!-- Add more number and operator buttons... -->
    </div>
</body>
```

### Step 5: Create the Calculator Class

Build the JavaScript logic with a Calculator class:

```javascript
class Calculator {
    constructor(previousOperandElement, currentOperandElement, sound) {
        this.previousOperandElement = previousOperandElement;
        this.currentOperandElement = currentOperandElement;
        this.sound = sound;
        this.clear();
    }

    playSound() {
        if (this.sound) {
            this.sound.currentTime = 0;
            this.sound.play().catch(() => {});
        }
    }

    clear() {
        this.currentOperand = '0';
        this.previousOperand = '';
        this.operation = undefined;
        this.updateDisplay();
    }

    appendNumber(number) {
        if (number === '.' && this.currentOperand.includes('.')) return;
        this.currentOperand = this.currentOperand === '0' && number !== '.' 
            ? number.toString() 
            : this.currentOperand + number;
        this.updateDisplay();
        this.playSound();
    }

    chooseOperation(operation) {
        if (this.previousOperand !== '') this.compute();
        this.operation = operation;
        this.previousOperand = this.currentOperand;
        this.currentOperand = '0';
        this.updateDisplay();
        this.playSound();
    }

    compute() {
        const prev = parseFloat(this.previousOperand);
        const current = parseFloat(this.currentOperand);
        if (isNaN(prev) || isNaN(current)) return;
        
        const operations = {
            '+': prev + current,
            '-': prev - current,
            '*': prev * current,
            '/': prev / current
        };
        
        this.currentOperand = operations[this.operation]?.toString() || '';
        this.operation = undefined;
        this.previousOperand = '';
        this.updateDisplay();
        this.playSound();
    }

    updateDisplay() {
        this.currentOperandElement.innerText = this.currentOperand;
        this.previousOperandElement.innerText = this.operation 
            ? `${this.previousOperand} ${this.operation}` 
            : '';
    }
}
```

### Step 6: Wire Up Event Listeners

Connect buttons to the calculator:

```javascript
const calculator = new Calculator(
    document.querySelector('[data-previous-operand]'),
    document.querySelector('[data-current-operand]'),
    new Audio('https://www.soundjay.com/buttons/beep-01a.mp3')
);

document.querySelectorAll('[data-number]').forEach(btn => {
    btn.addEventListener('click', () => calculator.appendNumber(btn.innerText));
});

document.querySelectorAll('[data-operator]').forEach(btn => {
    btn.addEventListener('click', () => calculator.chooseOperation(btn.innerText));
});

document.querySelector('[data-equals]').addEventListener('click', () => calculator.compute());
document.querySelector('[data-all-clear]').addEventListener('click', () => calculator.clear());
```

### Step 7: Add Keyboard Support

Enable keyboard input:

```javascript
document.addEventListener('keydown', e => {
    if (e.key >= '0' && e.key <= '9' || e.key === '.') calculator.appendNumber(e.key);
    if (['+', '-', '*', '/'].includes(e.key)) calculator.chooseOperation(e.key);
    if (e.key === 'Enter' || e.key === '=') calculator.compute();
    if (e.key === 'Backspace') calculator.delete();
    if (e.key === 'Escape') calculator.clear();
});
```

### Step 8: Deploy to Vercel

```bash
# Install Vercel CLI
npm i -g vercel

# Deploy
vercel --yes
```

---

## 🛠️ Tech Stack

- HTML5
- Tailwind CSS (CDN)
- Vanilla JavaScript

## 📜 License

MIT License

---

**Author:** [Muhammad Sajid Mahamud](https://github.com/sajidmahamud835)
