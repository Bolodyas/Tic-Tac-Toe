from PyQt5.QtWidgets import *
from PyQt5.QtCore import *
 
app = QApplication([])
main_win = QWidget()
button = QPushButton("Заново")
label = QLabel("статус")
 
switcher = QButtonGroup()
button0 = QRadioButton("0")
buttonX = QRadioButton("X")
switcher.addButton(button0, 0)
switcher.addButton(buttonX, 1)
 
buttons = []
layoutB = QGridLayout()
layoutB.setSpacing(0)
x, y = 0, 0
 
 
layout = QVBoxLayout()
layout.addWidget(button)
layout.addWidget(button0)
layout.addWidget(buttonX)
layout.addLayout(layoutB)
layout.addWidget(label)
main_win.setLayout(layout)
 
 
def start():
    switcher.button(0).setEnabled(True)
    switcher.button(1).setEnabled(True)
    switcher.button(1).setChecked(True)
    for i in range(9):
        buttons[i].setEnabled(True)
        buttons[i].setText("")
    label.setText("выберите игрока и ходите")
 
def process():
    button.sender().setText(switcher.button(switcher.checkedId()).text())
    button.sender().setEnabled(False)
 
    switcher.button(0).setEnabled(False)
    switcher.button(1).setEnabled(False)
    if switcher.checkedId() == 0:
        label.setText("ход игрока X")
        switcher.button(1).setChecked(True)
    else:
        label.setText("ход игрока 0")
        switcher.button(0).setChecked(True)
 
        winner = checkWinner()
        if winner != -1:
            finish(winner)
            return
 
        if not checkEnabled():
            finish()
 
def checkWinner():
    winPos = ((0, 1, 2),
                (3, 4, 5),
                (6, 7, 8),
                (0, 3, 6),
                (1, 4, 7),
                (2, 5, 8),
                (0, 4, 8),
                (2, 4, 6))
    for pos in winPos:
        if buttons[pos[0]].text() == buttons[pos[1]].text() == buttons[pos[2]].text() == "0":
            return 0
        if buttons[pos[0]].text() == buttons[pos[1]].text() == buttons[pos[2]].text() == "X":
            return 1
    return -1
 
def checkEnabled():
    for i in range(9):
        if buttons[i].isEnabled():
            return True
    return False
 
def finish(winner=-1):
    for i in range(9):
        buttons[i].setEnabled(False)
    if winner == 0:
        label.setText("игрок 0 выиграл")
    elif winner == 1:
        label.setText("игрок X выиграл")
    else:
        label.setText("ничья")
for i in range(9):
    button = QPushButton()
    buttons.append(button)
    button.setObjectName(str(i))
    button.clicked.connect(process)
    layoutB.addWidget(button, y, x)
    if (x == 2):
        x = 0
        y = y + 1
    else:
        x = x + 1
start()
button.clicked.connect(start)
 
main_win.show()
app.exec_()
