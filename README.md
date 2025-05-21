class Token:
    def __init__(self, value):
        self.value = value

def precedence(op):
    if op in ('+', '-'):
        return 1
    if op in ('*', '/'):
        return 2
    return 0

def toPostfix(expr):
    output = []
    stack = []
    tokens = expr.split()

    for token in tokens:
        if token.isnumeric():  # jika token adalah angka
            output.append(Token(token))
        elif token in ('+', '-', '*', '/'):
            while (stack and precedence(stack[-1]) >= precedence(token)):
                output.append(Token(stack.pop()))
            stack.append(token)
        elif token == '(':
            stack.append(token)
        elif token == ')':
            while stack and stack[-1] != '(':
                output.append(Token(stack.pop()))
            stack.pop()  # pop '(' dari stack

    while stack:
        output.append(Token(stack.pop()))

    return output

def evalPostfix(tokens):
    stack = []
    
    for token in tokens:
        if token.value.isnumeric():
            stack.append(int(token.value))
        else:
            right = stack.pop()
            left = stack.pop()
            if token.value == '+':
                stack.append(left + right)
            elif token.value == '-':
                stack.append(left - right)
            elif token.value == '*':
                stack.append(left * right)
            elif token.value == '/':
                stack.append(left / right)

    return stack[0]

# Contoh penggunaan
expr = "3 + 4 * 2 / ( 1 - 5 )"
postfix_tokens = toPostfix(expr)
result = evalPostfix(postfix_tokens)

print("Ekspresi Infix: ", expr)
print("Ekspresi Postfix: ", [token.value for token in postfix_tokens])
print("Hasil Evaluasi: ", result)


output
Ekspresi Infix:  3 + 4 * 2 / ( 1 - 5 )
Ekspresi Postfix:  ['3', '4', '2', '*', '1', '5', '-', '/', '+']
Hasil Evaluasi:  1.0
