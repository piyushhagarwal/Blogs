---
title: "Building a basic JSON parser in Rust"
seoTitle: "JSON Parser in Rust"
seoDescription: "Create a basic JSON parser in Rust that will simply tell if the input string is a valid JSON or not."
datePublished: Fri Feb 23 2024 17:52:33 GMT+0000 (Coordinated Universal Time)
cuid: clsyy9jp900020alagzes3hia
slug: building-a-basic-json-parser-in-rust
cover: https://cdn.hashnode.com/res/hashnode/image/upload/v1708710607115/b7b86eb3-cd37-4d97-bad3-49e3f0f64acd.png
tags: json, rust, json-parser

---

**JSON (JavaScript Object Notation)** is a widely used data interchange format that is easy for humans to read and write. In this tutorial, we will explore how to create a basic JSON parser in Rust that will simply tell if the input string is a valid JSON or not.

JSON consists of key-value pairs, where values can be strings, numbers, objects, arrays, booleans, or null.

```javascript
// Example of JSON
{
  "name": "John Doe",
  "age": 30,
  "isStudent": false,
  "grades": [90, 85, 92],
  "address": {
    "city": "New York",
    "zipCode": "10001"
  },
  "isEmployed": null
}
```

### Lexical analysis

Lexical analysis would be the first thing to undertake. It denotes splitting the string up into individual tokens.

Suppose we create an enum called `token_type` that informs us about the valid types a JSON can encompass.

```rust
pub enum TokenType {
    // Structural Tokens
    OpenBrace,      // {
    CloseBrace,     // }
    OpenBracket,    // [
    CloseBracket,   // ]
    Colon,          // :
    Comma,          // ,
    
    // Value Tokens
    String,         // "string"
    Number,         // 123 or 12.34
    True,           // true
    False,          // false
    Null,           // null
}
```

Now we create an enum called `Token` that comprises `token_type` and `value`.

```rust
pub struct Token{
    pub token_type: TokenType,
    pub value: String,
}
```

We will write a `lexer()` function that takes a string as input and returns a vector of `Token`. Let's illustrate with an example to clarify the expected output from our `lexer()` function.

```rust
// Example of JSON
{
  "name": "John Doe",
  "age": 30,
  "isStudent": false,
  "isEmployed": null
}

// Expected vector of tokens generated
[
    Token { token_type: OpenBrace, value: "{" },
    Token { token_type: String, value: "name" },
    Token { token_type: Colon, value: ":" },
    Token { token_type: String, value: "John Doe" },
    Token { token_type: Comma, value: "," },
    Token { token_type: String, value: "age" },
    Token { token_type: Colon, value: ":" },
    Token { token_type: Number, value: "30" },
    Token { token_type: Comma, value: "," },
    Token { token_type: String, value: "isStudent" },
    Token { token_type: Colon, value: ":" },
    Token { token_type: False, value: "false" },
    Token { token_type: Comma, value: "," },
    Token { token_type: String, value: "isEmployed" },
    Token { token_type: Colon, value: ":" },
    Token { token_type: Null, value: "null" },
    Token { token_type: CloseBrace, value: "}" }
]
```

`lexer()` function for generating a vector of tokens.

```rust
pub fn lexer(json: &str) -> Vec<Token> {
    let mut tokens = Vec::new();
    let mut current_pointer = 0;

    // Loop through the input JSON string
    while current_pointer < json.len() {
        // Check for opening curly brace '{'
        if json.chars().nth(current_pointer) == Some('{') {
            tokens.push(Token{token_type: TokenType::OpenBrace, value: String::from("{")});
            current_pointer += 1;
        }
        // Check for closing curly brace '}'
        else if json.chars().nth(current_pointer) == Some('}') {
            tokens.push(Token{token_type: TokenType::CloseBrace, value: String::from("}")});
            current_pointer += 1;
        }
        // Check for opening square bracket '['
        else if json.chars().nth(current_pointer) == Some('[') {
            tokens.push(Token{token_type: TokenType::OpenBracket, value: String::from("[")});
            current_pointer += 1;
        }
        // Check for closing square bracket ']'
        else if json.chars().nth(current_pointer) == Some(']') {
            tokens.push(Token{token_type: TokenType::CloseBracket, value: String::from("]")});
            current_pointer += 1;
        }
        // Check for colon ':'
        else if json.chars().nth(current_pointer) == Some(':') {
            tokens.push(Token{token_type: TokenType::Colon, value: String::from(":")});
            current_pointer += 1;
        }
        // Check for comma ','
        else if json.chars().nth(current_pointer) == Some(',') {
            tokens.push(Token{token_type: TokenType::Comma, value: String::from(",")});
            current_pointer += 1;
        }
        // Check for numeric values
        else if json.chars().nth(current_pointer).expect("Index out of bound").is_numeric() {
            let mut value = String::new();
            let mut i = current_pointer;
            // Collect numeric characters
            while json.chars().nth(i).expect("Index out of bound").is_numeric() {
                value.push(json.chars().nth(i).expect("Index out of bound"));
                i += 1;
            }
            tokens.push(Token{token_type: TokenType::Number, value});
            current_pointer = i;
        }
        // Check for the keyword 'true'
        else if json.chars().nth(current_pointer) == Some('t') {
            let mut value = String::new();
            let mut i = current_pointer;
            // Verify 'true' and update index
            if json.chars().nth(i) == Some('t') && json.chars().nth(i+1) == Some('r') && json.chars().nth(i+2) == Some('u') && json.chars().nth(i+3) == Some('e')  {
                i += 4;
                value = String::from("true");
            }
            tokens.push(Token{token_type: TokenType::True, value});
            current_pointer = i;
        }
        // Check for the keyword 'false'
        else if json.chars().nth(current_pointer) == Some('f') {
            let mut value = String::new();
            let mut i = current_pointer;
            // Verify 'false' and update index
            if json.chars().nth(i) == Some('f') && json.chars().nth(i+1) == Some('a') && json.chars().nth(i+2) == Some('l') && json.chars().nth(i+3) == Some('s') && json.chars().nth(i+4) == Some('e') {
                i += 5;
                value = String::from("false");
            }
            tokens.push(Token{token_type: TokenType::False, value});
            current_pointer = i;
        }
        // Check for the keyword 'null'
        else if json.chars().nth(current_pointer) == Some('n') {
            let mut value = String::new();
            let mut i = current_pointer;
            // Verify 'null' and update index
            if json.chars().nth(i) == Some('n') && json.chars().nth(i+1) == Some('u') && json.chars().nth(i+2) == Some('l') && json.chars().nth(i+3) == Some('l')  {
                i += 4;
                value = String::from("null");
            }
            tokens.push(Token{token_type: TokenType::Null, value});
            current_pointer = i;
        }
        // Check for double quotes indicating the start of a string
        else if json.chars().nth(current_pointer) == Some('"') {
            let mut value = String::new();
            let mut i = current_pointer + 1;
            // Collect characters until the closing double quote
            while json.chars().nth(i) != Some('"') {
                value.push(json.chars().nth(i).expect("Index out of bound"));
                i += 1;
            }
            tokens.push(Token{token_type: TokenType::String, value});
            current_pointer = i + 1;
        }
        // Ignore whitespace characters
        else if json.chars().nth(current_pointer) == Some(' ') || json.chars().nth(current_pointer) == Some('\n') || json.chars().nth(current_pointer) == Some('\t'){
            current_pointer += 1;
        }
        // If the character is not a valid JSON token, return an empty vector
        else {
            return Vec::new();
        }
    }
    tokens // Return the tokens vector
}
```

### Parsing

1. *Parsing JSON values*
    
    Once we have tokens, the next step is to parse the JSON values. In Rust, we use a recursive descent parser, which starts from the top level and recursively parses nested structures. We create a `parse_value` function to handle different types of JSON values: strings, numbers, booleans, null, objects, and arrays.
    
    ```rust
    // Function to parse individual values or in our case individual tokens
    pub fn parse_value(tokens : &Vec<Token>, current_pointer : &mut usize) -> bool{
        let token = &tokens[*current_pointer];
        if token.token_type == TokenType::String {
            true
        } else if token.token_type == TokenType::Number {
            true
        } else if token.token_type == TokenType::True {
            true
        } else if token.token_type == TokenType::False {
            true
        } else if token.token_type == TokenType::Null {
            true
        } else if token.token_type == TokenType::OpenBrace {
            parse_object(tokens, current_pointer)
        } else if token.token_type == TokenType::OpenBracket {
            parse_array(tokens, current_pointer)
        } else{
            false
        }
    }
    ```
    
2. *Parsing JSON Objects*
    
    Objects in JSON are key-value pairs enclosed in curly braces. We create a `parse_object` function to handle the parsing of JSON objects. It ensures that each key is a string followed by a colon and a valid value. The function also handles commas between key-value pairs and checks for proper closure with a closing brace.
    
    ```rust
    // Fuction to parse and check if the object is valid
    pub fn parse_object(tokens : &Vec<Token>, current_pointer : &mut usize) -> bool{
        *current_pointer += 1; // Skip the open brace
        while *current_pointer < tokens.len() && tokens[*current_pointer].token_type != TokenType::CloseBrace{
            
            if tokens[*current_pointer].token_type != TokenType::String{
                return false; // Key is not in the form of a string
            }
    
            *current_pointer += 1; // Skip the key
    
            if tokens[*current_pointer].token_type != TokenType::Colon{
                return false; // Key is not followed by a colon
            }
    
            *current_pointer += 1; // Skip the colon
    
            if !parse_value(tokens, current_pointer){
                return false;
            }
    
            *current_pointer += 1; // Skip the value
            
            // If the next token is a comma and after the comma there is closing brace, return false (e.g. {"key": "value",})
            if *current_pointer < tokens.len() && tokens[*current_pointer].token_type == TokenType::Comma && tokens[*current_pointer + 1].token_type == TokenType::CloseBrace{
                return false;
            }
    
            // If the next token is a comma, skip it
            if *current_pointer < tokens.len() && tokens[*current_pointer].token_type == TokenType::Comma{
                *current_pointer += 1; // Skip the comma
            }
        }
    
        if *current_pointer < tokens.len() && tokens[*current_pointer].token_type != TokenType::CloseBrace{
            return false; // If the object is not closed
        }
    
        *current_pointer += 1; // Skip the closing brace
    
        true // If the object is valid and current_pointer is at the closing brace
    }
    ```
    
3. Parsing JSON arrays
    
    Arrays in JSON are ordered lists of values enclosed in square brackets. We create a `parse_array` function to handle the parsing of JSON arrays. Similar to parsing objects, this function ensures valid values, handles commas between elements, and checks for proper closure with a closing bracket.
    
    ```rust
    // Fuction to parse and check if the array is valid
    pub fn parse_array(tokens : &Vec<Token>, current_pointer : &mut usize) -> bool{
        *current_pointer += 1; // Skip the open bracket
        while *current_pointer < tokens.len() && tokens[*current_pointer].token_type != TokenType::CloseBracket{
            if !parse_value(tokens, current_pointer){
                return false;
            }
    
            *current_pointer += 1; // Skip the value
            
            // If the next token is a comma and after the comma there is closing bracket, return false (e.g. ["value",])
            if *current_pointer < tokens.len() && tokens[*current_pointer].token_type == TokenType::Comma && tokens[*current_pointer + 1].token_type == TokenType::CloseBracket{
                return false;
            }
    
            // If the next token is a comma, skip it
            if *current_pointer < tokens.len() && tokens[*current_pointer].token_type == TokenType::Comma{
                *current_pointer += 1; // Skip the comma
            }
        }
    
        if *current_pointer < tokens.len() && tokens[*current_pointer].token_type != TokenType::CloseBracket{
            return false; // If the array is not closed
        }
    
        // If the json consists only array
        if *current_pointer == &tokens.len() - 1 {
            *current_pointer += 1;
        }
    
        true // If the array is valid
    }
    ```
    
4. *Putting all together*
    
    Finally, we combine the lexer and parser functions to create a complete JSON parser. The `parse` function takes a vector of tokens and verifies whether the input string adheres to valid JSON syntax.
    
    ```rust
    // Function to parse the tokens vector and check if the JSON is valid
    pub fn parse(tokens: &Vec<Token>) -> bool {
        // Check if the tokens vector is empty
        if tokens.len() == 0 {
            // If the JSON is empty, return false
            return false;
        }
    
        let mut current_pointer = 0;
    
        // Call the recursive parse_value function to start parsing the JSON
        let result = parse_value(&tokens, &mut current_pointer);
        
        // Return true if parsing is successful and the entire tokens vector is consumed
        result && current_pointer == tokens.len()
    }
    ```
    

For a detailed walkthrough and the complete Rust code, you can refer to this [GitHub repository](https://github.com/piyushhagarwal/Coding_Challenges_Rust/tree/main/json_parser).

### **Conclusion**

Building a JSON parser involves breaking down the task into smaller steps: tokenizing the input string, parsing different types of values, handling objects and arrays, and ensuring proper closure.

Happy coding!