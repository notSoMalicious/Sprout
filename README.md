# ***JAVASCRIPT***
I choosed JS (+ flow) because I clearly understand that if I choose another language I will spend much more time.

I spent only 3 hours.
| Time         | Description |
|-----------------|-------------|
| `1st hour` | to understand problem |
| `2nd`  | solving "H" part |
| `3rd` | solving "K" part && several comments |

To be honest with you, I believe that this task should take 1 day (3 hours to alfa version, 2 hours for refactoring, 2 hours for testing, 1 hour to implement in React (implenetation on React probably take much more time. It depends on how good I know your stack)).

```
const DEFAULT_BOOLEAN_CASES = [
    {
        A: true,
        B: true,  // true -> no need to reverse value
        C: false, // false -> need to reverse value
        formula: 'M', // if it`s a match by 'abc' use formula as boolean answer
    },
    {
        A: true,
        B: true,
        C: true,
        formula: 'P',
    },
    {
        A: false,
        B: true,
        C: true,
        formula: 'T',
    },
];

const DEFAULT_EVALUATING_MAP = { // !!!sould be refactored (DRY power :D)!!!
    M: (D: number, E: number, F: number): number => D + (D * E / 10),
    P: (D: number, E: number, F: number): number => D + (D * (E - F) / 25.5),
    T: (D: number, E: number, F: number): number => D - (D * F / 30),
};

type Formula = 'M' | 'P' | 'T';
type H = typeof undefined | Formula;
type K = typeof undefined | number;
type BooleanCase = {|
    A: boolean,
    B: boolean,
    C: boolean,
    formula: Formula,
|};
type EvaluateCase = {
    [formula: Formula]: (D: number, E: number, F: number) => number
};

function assignment (
    A: boolean,
    B: boolean,
    C: boolean,
    D: number,
    E: number,
    F: number,
    customBooleanRules?: Array<BooleanCase> = [], // Array shape because it can be several rules 
    customNumberRules?: EvaluateCase, // Object shape because every formula has unique symbol
): {| H: H, K: K |} {
    const result = { // !!!sould be refactored (undefined)!!!
        H: undefined, // boolean
        K: undefined, // number
    };

    const booleanRules = customBooleanRules.concat(DEFAULT_BOOLEAN_CASES);
    booleanRules.some(ruleSetup => {
        const temp = { // !!!sould be refactored (redundant var)!!!
            A,
            B,
            C,
        };

        // !!!sould be refactored (mutation)!!!
        if (!ruleSetup.A) temp.A = !A;
        if (!ruleSetup.B) temp.B = !B;
        if (!ruleSetup.C) temp.C = !C;
 
        if (temp.A && temp.B && temp.C) {
            result.H = ruleSetup.formula;
            return true;
        }
    });

    if (!result.H) throw new Error('[other] => [error]')

    const evaluatingMap = { ...DEFAULT_EVALUATING_MAP, ...customNumberRules }
    result.K = evaluatingMap[result.H](D, E, F)

    return result;
}

console.log(
    assignment(
        true,
        true,
        false,
        1.5,
        1,
        1,
        [{
            A: true,
            B: true,
            C: false,
            formula: 'T',
        }],
        {
            T: (D, E, F) => F + D + (D * E / 100)
        },
    )
);
```
