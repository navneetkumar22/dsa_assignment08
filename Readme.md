# PPT - DSA Assignment 08

## Answer 1:
```js
const minimumDeleteSum = (s1, s2) => {
    const m = s1.length;
    const n = s2.length;
    const f = Array.from({ length: m + 1 }, () => Array(n + 1).fill(0));
    for (let i = 1; i <= m; ++i) {
        f[i][0] = f[i - 1][0] + s1[i - 1].charCodeAt(0);
    }
    for (let j = 1; j <= n; ++j) {
        f[0][j] = f[0][j - 1] + s2[j - 1].charCodeAt(0);
    }
    for (let i = 1; i <= m; ++i) {
        for (let j = 1; j <= n; ++j) {
            if (s1[i - 1] === s2[j - 1]) {
                f[i][j] = f[i - 1][j - 1];
            } else {
                f[i][j] = Math.min(
                    f[i - 1][j] + s1[i - 1].charCodeAt(0),
                    f[i][j - 1] + s2[j - 1].charCodeAt(0),
                );
            }
        }
    }
    return f[m][n];
};
```

<br/>

## Answer 2:
```js
const checkValidString = (str) => {
    let balance = 0
    for (const c of str) {
        if (c === '(' || c === '*') {
            balance += 1
        } else if (c === ')') {
            balance -= 1
        }
        if (balance < 0) {
            return false
        }
    }
    balance = 0
    for (let i = str.length - 1; i >= 0; i--) {
        const c = str[i]
        if (c === ')' || c === '*') {
            balance += 1
        } else if (c === '(') {
            balance -= 1
        }
        if (balance < 0) {
            return false
        }
    }
    return true
}
```

<br/>

## Answer 3:
```js
const minSteps = (W1, W2) => {
    let m = W1.length, n = W2.length
    if (m < n) {
        [W1, W2, m, n] = [W2, W1, n, m];
    }

    let WA1 = W1.split("");
    let WA2 = W2.split("");
    let dpLast = new Uint16Array(n + 1);
    let dpCurr = new Uint16Array(n + 1);

    for (let i = 0; i < m; i++) {

        for (let j = 0; j < n; j++)
            dpCurr[j + 1] = WA1[i] === WA2[j] ? dpLast[j] + 1 : Math.max(dpCurr[j], dpLast[j + 1]);
        [dpLast, dpCurr] = [dpCurr, dpLast];
    }
    return m + n - 2 * dpLast[n]
};
```

<br/>

## Answer 5:
```js
const compressChars = (chars) => {
    let count = 1;

    for (let i = 1; i <= chars.length; i++) {
        if (chars[i] == chars[i - 1]) {

            //start iterating from second character and compare it with the previous character

            count++;

            //if same character is found, increase count by 1 (note 1)

        } else {
            if (count > 1) {
                let countArr = count.toString().split('');

                //convert count from integer to string (note 2). And "split()" (note 3) is to make sure that when count is a two digit integer, for example, 12, it will be split into multiple characters: "1" and "2" then stored in the array (part of the requirement).

                let deletedElement = chars.splice(i - count + 1,
                    count - 1, ...countArr);

                /*Delete duplicated elements and replace them with character count (note 4)*/

                i = i - deletedElement.length + countArr.length;

                /*reset the index to the start of new character. In test case:
                ["a","a","a","b","b"], the index for the three "a" are 0, 1, 2 and index for the first "b" is 3. Once we compress all the "a" and modify the array with the count, we will get a new array ["a","2","b","b"].
                And in this new array, the index of the first "b" becomes 2. This solution uses "i - deletedElement.length + countArr.length" to reset i to the new character.*/

            }
            count = 1;

            /*reset the count back to 1, so that counting starts again with new character*/

        }
    }

    return chars.length;

};
```

<br/>

## Answer 6:
```js
const findAnagrams = (s, p) => {
    // if length of s less than length of p - return empty list - End "if"

    if (s.length < p.length) {
        return [];
    }

    const MAX = 26; //26 represent the english alphabet
    const M = p.length;
    const N = s.length;

    //Create empty array arrP to hold occurance of letters for pattern P
    const arrP = Array(26).fill(0);

    //Create empty array arrS to hold occurance of letters for string S
    const arrS = Array(26).fill(0);

    // Create the base value for UTF-16 code unit for letter 'a' as the start of the alphabet
    const base = "a".charCodeAt(0);

    // This function returns true if contents of arr1[] and arr2[] are same, otherwise false.

    function compare(arrS, arrP) {
        for (let i = 0; i < MAX; i++) {
            if (arrS[i] !== arrP[i]) {
                return false;
            }
        }
        return true;
    }

    // for loop in pattern P & string S and populate array arrP & arrS at the proper index based on the UTF-16 index value up to the length of pattern
    for (let i = 0; i < M; i++) {
        const indexP = p.charCodeAt(i) - base;
        arrP[indexP]++;
        const indexS = s.charCodeAt(i) - base;
        arrS[indexS]++;
    }

    //create and empty array to hold the indexes of the anagrams when found in the string
    let result = [];

    // starting at index M in string create for loop in string S and populate array arrS at the proper index based on the UTF-16 index value
    for (let i = M; i <= N; i++) {
        // if arrS & arrP are equal at the index of the start of the anagram to result[]
        if (compare(arrS, arrP)) {
            result.push(i - M);
        }

        // Remove the first character of previous window
        arrS[s.charCodeAt(i - M) - base]--;

        // Add current character to current window
        arrS[s.charCodeAt(i) - base]++;
    }
    return result;
};
```

<br/>

## Answer 7:
```js
const decodeString = s => {

    if (s.length === 1) {
        if (Number.isInteger(+s))
            return "";
        else
            return s;
    }

    const stack = [];

    for (let i = 0; i < s.length; i++) {
        if (s[i] === ']') {
            let str = '';

            while (true) {
                const char = stack.pop()
                if (char === '[')
                    break;
                str = char + str;
            }

            let n = '';

            while (Number.isInteger(+stack[stack.length - 1])) {
                const number = stack.pop();
                n = number + n;
            }
            stack.push(str.repeat(+n));
        }
        else
            stack.push(s[i])
    }
    return stack.join('')
};
```

<br/>

## Answer 8:
```js
const buddyStrings = (s, goal) => {
    let differences = [];
    let sSet = new Set(s);
    
    for (let i = 0; i < s.length; i++) {
        if (s[i] !== goal[i]);
        differences.push([s[i], goal[i]]);
    }

    if (s.length !== goal.length)
        return false;

    if (s === goal)
        return sSet.size < s.length;

    if (differences.length === 2)
        return differences[0].toString() === differences[1].reverse().toString();

    return false;
};
```