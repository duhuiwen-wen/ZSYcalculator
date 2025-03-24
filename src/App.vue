<template>
    <div class="calculator">
        <div class="display-container">
            <div class="expression">{{ expression }}</div>
            <div class="display">{{ display }}</div>
        </div>
        <div class="buttons">
            <button v-for="btn in buttons" :key="btn" @click="handleClick(btn)" :class="{
                'operator': ['+', '-', '*', '/', '='].includes(btn),
                'clear': btn === 'C',
                'number': !['+', '-', '*', '/', '=', 'C'].includes(btn)
            }">
                {{ btn }}
            </button>
        </div>
    </div>
</template>

<script>
export default {
    name: 'App',
    data() {
        return {
            display: '0',
            expression: '',
            currentNumber: '',
            previousNumber: '',
            operation: null,
            buttons: ['C', '7', '8', '9', '/', '4', '5', '6', '*', '1', '2', '3', '-', '0', '.', '=', '+']
        }
    },
    methods: {
        calculateResult() {
            if (this.previousNumber && this.operation && this.currentNumber) {
                const prev = parseFloat(this.previousNumber)
                const current = parseFloat(this.currentNumber)
                let result

                switch (this.operation) {
                    case '+':
                        result = prev + current
                        break
                    case '-':
                        result = prev - current
                        break
                    case '*':
                        result = prev * current
                        break
                    case '/':
                        result = prev / current
                        break
                }

                return result.toString()
            }
            return this.currentNumber
        },
        handleClick(value) {
            if (value === 'C') {
                // 清空所有数据
                this.display = '0'
                this.expression = ''
                this.currentNumber = ''
                this.previousNumber = ''
                this.operation = null
                return
            }
            if (value >= '0' && value <= '9' || value === '.') {
                if (this.display === '0' && value !== '.') {
                    this.display = value
                } else {
                    this.display += value
                }
                this.currentNumber = this.display
                // 实时更新表达式
                if (this.operation) {
                    this.expression = `${this.previousNumber} ${this.operation} ${this.currentNumber}`
                }
            } else if (['+', '-', '*', '/'].includes(value)) {
                if (this.previousNumber && this.operation) {
                    // 如果已经有之前的计算，先计算之前的结果
                    this.display = this.calculateResult()
                    this.currentNumber = this.display
                }
                this.expression = `${this.display} ${value}`
                this.previousNumber = this.display
                this.operation = value
                this.display = '0'
            } else if (value === '=') {
                if (this.previousNumber && this.operation) {
                    const result = this.calculateResult()
                    this.expression = `${this.expression}`
                    this.display = result
                    this.currentNumber = result
                    this.previousNumber = ''
                    this.operation = null
                }
            }
        }
    }
}
</script>

<style>
.calculator {
    width: 375px;
    height: auto;
    margin: 0 auto;
    background-color: #000000;
    border-radius: 40px;
    padding: 20px;
    display: flex;
    flex-direction: column;
    box-sizing: border-box;
}

.display-container {
    flex: 1;
    background-color: transparent;
    padding: 40px 20px;
    margin-bottom: 20px;
    display: flex;
    flex-direction: column;
    justify-content: flex-end;
    align-items: flex-end;
}

.expression {
    text-align: right;
    font-size: 24px;
    color: #999;
    min-height: 30px;
    margin-bottom: 8px;
    font-family: -apple-system, BlinkMacSystemFont, "Segoe UI", Roboto, sans-serif;
}

.display {
    text-align: right;
    font-size: 80px;
    color: white;
    min-height: 100px;
    word-wrap: break-word;
    font-family: -apple-system, BlinkMacSystemFont, "Segoe UI", Roboto, sans-serif;
    font-weight: 300;
}

.buttons {
    display: grid;
    grid-template-columns: repeat(4, 1fr);
    gap: 12px;
    padding: 10px;
    margin-bottom: 10px;
}

button {
    width: 70px;
    height: 70px;
    border-radius: 50%;
    border: none;
    font-size: 32px;
    cursor: pointer;
    transition: filter 0.2s;
    font-family: -apple-system, BlinkMacSystemFont, "Segoe UI", Roboto, sans-serif;
    margin: 0 auto;
}

button:active {
    filter: brightness(130%);
}

.number {
    background-color: #333333;
    color: white;
}

.operator {
    background-color: #ff9f0a;
    color: white;
    font-weight: 500;
}

.clear {
    background-color: #a5a5a5;
    color: black;
    font-weight: 500;
}

/* 调整等号按钮样式 */
button:nth-child(16) {
    background-color: #ff9f0a;
    color: white;
    font-weight: 500;
}

/* 调整零按钮样式 */
button:nth-child(14) {
    grid-column: span 2;
    width: 152px;
    border-radius: 35px;
    text-align: left;
    padding-left: 28px;
}
</style>