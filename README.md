Design a lexical Analyzer for given language should ignore the redundant spaces, tabs and new lines and ignore comments using C


#include <stdio.h>
#include <ctype.h>
#include <string.h>

#define MAX_TOKEN_LENGTH 100

// Function prototypes
void removeComments(char *code);
void removeWhitespace(char *code);
void analyzeTokens(char *code);

int main() {
    char code[] = "/* This is a comment */ int main() { int a = 10; // Another comment\n printf(\"Hello, World!\"); \n return 0; }";

    printf("Original Code:\n%s\n\n", code);

    removeComments(code);
    removeWhitespace(code);
    analyzeTokens(code);

    return 0;
}

// Function to remove comments
void removeComments(char *code) {
    char *p = code, *q = code;
    int inComment = 0;

    while (*p) {
        if (inComment) {
            if (*p == '*' && *(p + 1) == '/') {
                inComment = 0;
                p += 2;
            } else {
                p++;
            }
        } else {
            if (*p == '/' && *(p + 1) == '*') {
                inComment = 1;
                p += 2;
            } else if (*p == '/' && *(p + 1) == '/') {
                while (*p && *p != '\n') p++;
            } else {
                *q++ = *p++;
            }
        }
    }
    *q = '\0';
}

// Function to remove redundant spaces, tabs, and new lines
void removeWhitespace(char *code) {
    char *p = code, *q = code;
    int inWhitespace = 0;

    while (*p) {
        if (isspace(*p)) {
            if (!inWhitespace) {
                *q++ = ' ';
                inWhitespace = 1;
            }
        } else {
            *q++ = *p;
            inWhitespace = 0;
        }
        p++;
    }
    *q = '\0';
}

// Function to analyze tokens
void analyzeTokens(char *code) {
    char token[MAX_TOKEN_LENGTH];
    int index = 0;

    printf("Tokens:\n");
    while (*code) {
        if (isalnum(*code) || *code == '_') {
            token[index++] = *code;
        } else {
            if (index > 0) {
                token[index] = '\0';
                printf("%s\n", token);
                index = 0;
            }
            if (!isspace(*code)) {
                printf("%c\n", *code);
            }
        }
        code++;
    }
    if (index > 0) {
        token[index] = '\0';
        printf("%s\n", token);
    }
}
OUTPUT:
Original Code:
/* This is a comment */ int main() { int a = 10; // Another comment
 printf("Hello, World!"); 
 return 0; }

Tokens:
int
main
(
)
{
int
a
=
10
;
printf
(
"
Hello
,
World
!
"
)
;
return
0
;
}



