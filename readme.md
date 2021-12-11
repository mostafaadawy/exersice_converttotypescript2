Generics and the Promise Return Type

To fully understand how promises are typed in TypeScript, you need to understand Generics. A generic is a way to write a function that is reusable across different types. You may wonder, why not just use any? Well, any allows for any type to go in, and any type to come out. Using a generic means a number goes in and a number comes out or a string goes in and a string comes out.

Generics introduce the Type Variable. Rather than being a variable that accepts values, it's a variable that accepts types and is denoted with angle brackets myFunc<T>. It's common to see a capital T being used at the type in the generic to denote the use of a type.

// Typed Function
const getItem = (arr: number[]):number => {
  return arr[1];
}

// Generic Function
const getItem = <T>(arr: T[]):T => {
  return arr[1];
};

In the first function, we have a function that takes in a number array and outputs the second number of the array. But what if we don't want to work with numbers? What if we want to work with strings? Well, we would need to create a second function. Or, we can use a generic, and whatever type we use when we call the function will translate to its return as well.
Using Generic Functions

Implicit typing

getItem([1, 2, 3]); // Implicitly typed as number 

Explicit typing

getItem<number>([1,2,3]); // Explicitly typed as a number

So where does this leave us with Promises? You may or may not have noticed this, but when working with Promises, Promises is capitalized and it uses the new keyword to create one new Promise. That means that Promise is actually a constructor function (or class if using post ES6 terminology). Promises are built-in objects that have their own properties and methods.

The return type of an asynchronous function using async/await syntax is always a Promise. Promises in TypeScript take advantage of generics. This means we can explicitly state what type of Promise should be returned.

const myFunc = async ():Promise<void> => { // do stuff };

Asynchronous TypeScript
Promise Chaining VS Async/Await

This demo looks at how an asynchronous function that uses a Promise constructor can be typed, taking advantage of the code editors IntelliSense to guide the TypeScript process.

With a function that returns a Promise, use the generic type variable to state what type of Promise should be returned. Promise<number>

This example types the function arguments (step) as unknown via IntelliSense's recommendation requiring the use of type assertion when step is used in order for TypeScript to safely type the nextStep variable. Since nextStep is resolved as a number, the function returns a Promise of type number.

Rather than typing step as unknown, it would have been possible to type step as number, avoiding the need for type assertion later on. IntelliSense is very helpful and will likely give you an accurate option for the type, but it doesn't mean you can't do a better job.

Once we have our functions typed, we can then look at our Promise chain to type our callback arguments. In the case of this demonstration, we know the result should be typed as a number.

For our Async/Await syntax, we're able to type our functions the same way. Once we reach our async/await syntax, we're able to give the function a return type. We know that async/await syntax always returns a Promise. In this case, it returns a Promise since there is no return statement.

Trying to add a return statement once the return type has been declared as void results in IntelliSense prompting an error that 'number' is not assignable to type 'void'.
New Terms
Term 	Definition
Generic 	A generic is a way to write a function that is reusable across different types




Getting Started
First Steps

    Run npm run build to see initial typescript errors.
    Run node main to see how the application should function.
    Review the code

To Complete The Exercise

    Go through main.ts file type all function parameters, function returns, and class variables.
    Pay close attention to promise return types and input from the user which should have used parseIntto parse as numbers.
    Compile your TypeScript and run the application.


Solution Code

// Import readline module for getting input from console
// Find more here: https://nodejs.org/api/readline.html#readline_readline
import readline from 'readline';

// define question/output interface
const rl = readline.createInterface({
  // readable stream
  input: process.stdin,
  // writeable stream
  output: process.stdout
});

// Create questions for STDIN Input from console.
const menuQ = (): Promise<unknown> => {
  return new Promise((resolve, reject) => {
    // (readable, writeable from readline interface)
    try {
      rl.question('Your choice: ', (answer: unknown): void => {
        resolve(answer);
      });
    } catch (error) {
      reject();
    }
  });
};

const milkQ = (): Promise<unknown> => {
  return new Promise((resolve, reject) => {
    try {
      rl.question('How many cups of milk to add? ', (answer: unknown): void => {
        resolve(answer);
      });
    } catch (error) {
      reject();
    }
  });
};

const espressoQ = (): Promise<unknown> => {
  return new Promise((resolve, reject) => {
    try {
      rl.question(
        'How many shots of espresso to add? ',
        (answer: unknown): void => {
          resolve(answer);
        }
      );
    } catch (error) {
      reject();
    }
  });
};

const peppermintQ = (): Promise<unknown> => {
  return new Promise((resolve, reject) => {
    try {
      rl.question(
        'How many shots of peppermint to add? ',
        (answer: unknown): void => {
          resolve(answer);
        }
      );
    } catch (error) {
      reject();
    }
  });
};

// Create parent class Mocha
class Mocha {
  milk: number;
  shot: number;
  chocolateType: string;

  constructor() {
    this.milk = 1;
    this.shot = 1;
    this.chocolateType = 'dark';
  }
  // list the ingredients of the mocha
  prepare(): void {
    console.log('Your', this.chocolateType, ' Chocolate Mocha Ingredients:');
    console.log(this.chocolateType, ' chocolate');
    console.log('Cups of milk: ', this.milk);
    console.log('Cups of espresso: ', this.shot, '\n\n');
  }
}

// inherits from Mocha
class WhiteChocolateMocha extends Mocha {
  chocolateType = 'White';
}
// inherits from Mocha
class DarkChocolateMocha extends Mocha {
  chocolateType = 'Dark';
}
// inherits from Mocha
class PeppermintMocha extends Mocha {
  // add peppermint property
  peppermintSyrup: number;
  constructor() {
    // include super to pull in parent constructor
    super();
    this.peppermintSyrup = 1;
  }
  // Overrides Mocha prepare with additional statements
  prepare(): void {
    console.log('Your Peppermint Mocha Ingredients:');
    console.log('Dark chocolate');
    console.log('Cups of milk: ', this.milk);
    console.log('Cups of espresso: ', this.shot);
    console.log('Pumps of peppermint: ', this.peppermintSyrup, '\n\n');
  }
}

// display menu and return selected menu item
const showMenu = async (): Promise<number> => {
  console.log(
    'Select Mocha from menu: \n',
    '1: Create White Chocolate Mocha \n',
    '2: Create Dark Chocolate Mocha \n',
    '3: Create Peppermint Mocha\n',
    '0: Exit\n'
  );
  const qMenu = await menuQ();
  return qMenu as number;
};

// User questions
const userOptions = async (
  mochaObject: Mocha | PeppermintMocha
): Promise<void> => {
  const milkPicked = ((await milkQ()) as unknown) as string;
  const milkChoice: number = parseInt(milkPicked);
  const espPicked = ((await espressoQ()) as unknown) as string;
  const espChoice: number = parseInt(espPicked);
  // If peppermint mocha
  if (mochaObject instanceof PeppermintMocha) {
    const pepPicked = ((await peppermintQ()) as unknown) as string;
    const pepChoice: number = parseInt(pepPicked);
    mochaObject.peppermintSyrup = pepChoice;
  }

  mochaObject.milk = milkChoice;
  mochaObject.shot = espChoice;
  mochaObject.prepare();
};

const main = (): void => {
  let menuChoice = 0;
  const buildMocha = async (): Promise<void> => {
    do {
      const optionPicked = ((await showMenu()) as unknown) as string;
      menuChoice = parseInt(optionPicked);
      switch (menuChoice) {
        case 0: {
          break;
        }
        case 1: {
          const whiteMocha = new WhiteChocolateMocha();
          await userOptions(whiteMocha);
          break;
        }
        case 2: {
          const darkMocha = new DarkChocolateMocha();
          await userOptions(darkMocha);
          break;
        }
        case 3: {
          const peppermintMocha = new PeppermintMocha();
          await userOptions(peppermintMocha);
          break;
        }
        default: {
          console.log('Option invalid, please choose from menu.');
          break;
        }
      }
    } while (menuChoice != 0);
    // end readline process
    rl.close();
  };
  buildMocha();
};

main();

