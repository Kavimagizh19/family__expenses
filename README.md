// Family Expenses Calculator - Easter Eggs
// Add these to your ServiceNow client script

// Easter Egg 1: The "Coffee" Detector
function checkExpenseDescription(description) {
    var coffeeVariants = ['coffee', 'caffeine', 'starbucks', 'latte', 'espresso'];
    var lowerDesc = description.toLowerCase();
    
    if (coffeeVariants.some(word => lowerDesc.includes(word))) {
        var coffeeCount = parseInt(g_form.getValue('coffee_counter') || '0') + 1;
        g_form.setValue('coffee_counter', coffeeCount);
        
        if (coffeeCount === 1) {
            g_form.addInfoMessage('☕ First coffee expense logged. Your productivity thanks you.');
        } else if (coffeeCount === 5) {
            g_form.addInfoMessage('☕☕☕☕☕ That\'s 5 coffees! You\'re officially a caffeine connoisseur.');
        } else if (coffeeCount === 10) {
            g_form.addInfoMessage('🏆 ACHIEVEMENT UNLOCKED: Coffee Addict Level 10');
        }
    }
}

// Easter Egg 2: The "Nice" Amount (69 or 420)
function checkNiceAmount(amount) {
    if (amount == 69 || amount == 69.00) {
        g_form.addInfoMessage('Nice. 😏');
    } else if (amount == 420 || amount == 420.00) {
        g_form.addInfoMessage('Nice. 🌿');
    } else if (amount == 404) {
        g_form.addErrorMessage('Error 404: Money not found 💸');
    } else if (amount == 1337) {
        g_form.addInfoMessage('L33T H4X0R detected! 👾');
    }
}

// Easter Egg 3: Suspiciously Round Numbers
function checkSuspiciouslyRound(amount) {
    if (amount >= 1000 && amount % 1000 === 0) {
        g_form.addWarningMessage('🤔 That\'s suspiciously round... Did someone forget to save the receipt?');
    }
}

// Easter Egg 4: Friday Expense Detector
function checkFridayExpense(expenseDate) {
    var date = new Date(expenseDate);
    if (date.getDay() === 5) { // Friday
        var hour = date.getHours();
        if (hour >= 17) {
            g_form.addInfoMessage('🍺 Friday evening expense detected. We won\'t ask questions.');
        }
    }
}

// Easter Egg 5: The Konami Code (for ServiceNow form)
var konamiSequence = ['up', 'up', 'down', 'down', 'left', 'right', 'left', 'right', 'b', 'a'];
var konamiIndex = 0;

document.addEventListener('keydown', function(e) {
    var key;
    switch(e.keyCode) {
        case 38: key = 'up'; break;
        case 40: key = 'down'; break;
        case 37: key = 'left'; break;
        case 39: key = 'right'; break;
        case 66: key = 'b'; break;
        case 65: key = 'a'; break;
        default: key = null;
    }
    
    if (key === konamiSequence[konamiIndex]) {
        konamiIndex++;
        if (konamiIndex === konamiSequence.length) {
            g_form.addInfoMessage('🎮 KONAMI CODE ACTIVATED! Budget increased by $30... just kidding. 😄');
            konamiIndex = 0;
        }
    } else {
        konamiIndex = 0;
    }
});

// Easter Egg 6: Palindrome Date Detector
function checkPalindromeDate(dateStr) {
    var cleaned = dateStr.replace(/[-\/]/g, '');
    var reversed = cleaned.split('').reverse().join('');
    if (cleaned === reversed) {
        g_form.addInfoMessage('🔄 Palindrome date detected! The expense gods smile upon you.');
    }
}

// Main onChange function for Amount field
function onAmountChange() {
    var amount = parseFloat(g_form.getValue('amount'));
    if (!isNaN(amount)) {
        checkNiceAmount(amount);
        checkSuspiciouslyRound(amount);
    }
}

// Main onChange function for Description field
function onDescriptionChange() {
    var description = g_form.getValue('description');
    if (description) {
        checkExpenseDescription(description);
    }
}

// Main onChange function for Date field
function onDateChange() {
    var expenseDate = g_form.getValue('expense_date');
    if (expenseDate) {
        checkFridayExpense(expenseDate);
        checkPalindromeDate(expenseDate);
    }
}
