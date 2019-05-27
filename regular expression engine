#include <stdio.h>
#include <stdlib.h>
#include <string.h>
#include <malloc.h>
#define bool char
#define true 1
#define false 0
#define contains equalElem
#define NULL_CHAR 1
#define AUX 2
#define CAT 3

typedef enum _Type Type;
typedef enum _GrammarSymbol GrammarSymbol;
typedef enum _MetaSymbol MetaSymbol;
typedef enum _ActionType ActionType;
typedef enum _MergeType MergeType;
typedef enum _PositionType PositionType;
typedef enum _StringType StringType;
typedef enum _RepeatMode RepeatMode;
typedef struct _Node Node;
typedef struct _Stack Stack;
typedef struct _List List;
typedef struct _Char Char;
typedef struct _Short Short;
typedef struct _TreeNode TreeNode;
typedef struct _Record Record;
typedef struct _Repeat Repeat;
typedef struct _Production Production;
typedef struct _Action Action;
typedef struct _DFAMap DFAMap;
typedef struct _DFAState DFAState;

enum _Type
{
	CHAR,
	SHORT,
	RECORD,
	_REPEAT,
	TREE_NODE,
	DFA_MAP,
	DFA_STATE,
	LIST,
	STACK
};

enum _GrammarSymbol
{
	REG,
	TERM,
	FACTOR,
	ATOM,
	REPEAT,
	LETTER_CLASS,
	LETTER_SET,
	SET_FACTOR,
	OR,
	LETTER,
	LEFT_PAR,
	RIGHT_PAR,
	LEFT_BRACE,
	NUM,
	RIGHT_BRACE,
	COMMA,
	QUESTION_MARK,
	PLUS,
	STAR,
	LEFT_BRACKET,
	RIGHT_BRACKET,
	CARET,
	DOT,
	DIGIT,
	NON_DIGIT,
	SPACE,
	NON_SPACE,
	WORD,
	NON_WORD,
	EN_DASH,
	END
};

enum _ActionType
{
	NONE,
	SHIFT_IN,
	REDUCE,
	GOTO,
	ACCEPT
};

enum _MergeType
{
	REPEAT_ALLOWED,
	REPEAT_BANNED
};

enum _PositionType
{
	FIRST,
	LAST
};

enum _StringType
{
	GENERAL,
	REG_EXPR
};

enum _RepeatMode
{
	ABSENT,
	FINITE,
	INFINITE
};

struct _Char
{
	Type type;
	char val;
};

struct _Short
{
	Type type;
	unsigned short val;
};

struct _Record
{
	Type type;
	unsigned short state;
	void *attr;
};

struct _Repeat
{
	Type type;
	RepeatMode mode;
	unsigned short num1;
	unsigned short num2;
};

struct _TreeNode
{
	Type type;
	char val;
	List *followpos;
	TreeNode *left;
	TreeNode *right;
};

struct _Production
{
	unsigned short head;
	unsigned short bodySize;
};

struct _Action
{
	unsigned short actionType;
	unsigned short id;
};

struct _DFAMap
{
	Type type;
	DFAState *gotoState;
	List *symbolList;
};

struct _DFAState
{
	Type type;
	List *mapList;
	bool accept;
};

struct _Node
{
	void *elem;
	Node *next;
};

struct _Stack
{
	Type type;
	Node *top;
};

struct _List
{
	Type type;
	Node *head;
	Node *tail;
};

void readModeStr();
bool processPrimitiveStr();
bool constructRepeatUnit();
bool parse();
TreeNode *insert();
unsigned short sonCount();
void printAbstractSyntaxTree();
void printProcessedStr();
char charByOctal();
char charByHexadecimal();
char extendChar();
int charType();
bool reduce();
bool isHexadecimalDigit();
void addToLetterList();
bool nullable();
List *POSITION();
void formFollowPosBy();
void generateDFA();
void reduceDFA();
void runDFA();
List **divide();
List **emptyGroups();
unsigned short groupId();
unsigned short noneEmptyGroupCount();
void clearGroups();
unsigned short nextGroupId();
void freeGroups();
void *clone();
void checkStr();
DFAState *Goto();
void resume();

Char *newChar();
Short *newShort();
Record *newRecord();
Repeat *newRepeat();
TreeNode *newTreeNode();
DFAMap *newDFAMap();
DFAState *newDFAState();
List *newList();
Stack *newStack();
List *charRange();
TreeNode *cloneTree();
bool equals();
void *equalElem();
unsigned short elemId();
void *nthElem();
void addList();
void mergeList();
void addElem();
void removeElem();
void freeElem();
void freeList();
void freeStack();
void freeTree();
unsigned short listSize();
void *stackElem();
void push();
void *pop();
void printElem();
void printList();

bool containerOnly = true;
bool listOrderConsidered;
FILE *fp;
char mode[1024];
TreeNode *tree;
Stack *stack;
List *dfa;
List *numList;
List *letterList;
unsigned short numCount;

const Action parsingTable[49][31] = 
{ {{GOTO, 1}, {GOTO, 2}, {GOTO, 3}, {GOTO, 4}, {NONE, }, {GOTO, 5}, {NONE, }, {NONE, }, {NONE, }, {SHIFT_IN, 6}, {SHIFT_IN, 7}, {NONE, }, {NONE, }, {NONE, }, {NONE, }, {NONE, }, {NONE, }, {NONE, }, {NONE, }, {SHIFT_IN, 8}, {NONE, }, {NONE, }, {SHIFT_IN, 9}, {SHIFT_IN, 10}, {SHIFT_IN, 11}, {SHIFT_IN, 12}, {SHIFT_IN, 13}, {SHIFT_IN, 14}, {SHIFT_IN, 15}, {NONE, }, {NONE, }},
  {{NONE, }, {NONE, }, {NONE, }, {NONE, }, {NONE, }, {NONE, }, {NONE, }, {NONE, }, {SHIFT_IN, 16}, {NONE, }, {NONE, }, {NONE, }, {NONE, }, {NONE, }, {NONE, }, {NONE, }, {NONE, }, {NONE, }, {NONE, }, {NONE, }, {NONE, }, {NONE, }, {NONE, }, {NONE, }, {NONE, }, {NONE, }, {NONE, }, {NONE, }, {NONE, }, {NONE, }, {ACCEPT, }},
  {{NONE, }, {NONE, }, {GOTO, 17}, {GOTO, 4}, {NONE, }, {GOTO, 5}, {NONE, }, {NONE, }, {REDUCE, 1}, {SHIFT_IN, 6}, {SHIFT_IN, 7}, {REDUCE, 1}, {NONE, }, {NONE, }, {NONE, }, {NONE, }, {NONE, }, {NONE, }, {NONE, }, {SHIFT_IN, 8}, {NONE, }, {NONE, }, {SHIFT_IN, 9}, {SHIFT_IN, 10}, {SHIFT_IN, 11}, {SHIFT_IN, 12}, {SHIFT_IN, 13}, {SHIFT_IN, 14}, {SHIFT_IN, 15}, {NONE, }, {REDUCE, 1}},
  {{NONE, }, {NONE, }, {NONE, }, {NONE, }, {NONE, }, {NONE, }, {NONE, }, {NONE, }, {REDUCE, 3}, {REDUCE, 3}, {REDUCE, 3}, {REDUCE, 3}, {NONE, }, {NONE, }, {NONE, }, {NONE, }, {NONE, }, {NONE, }, {NONE, }, {REDUCE, 3}, {NONE, }, {NONE, }, {REDUCE, 3}, {REDUCE, 3}, {REDUCE, 3}, {REDUCE, 3}, {REDUCE, 3}, {REDUCE, 3}, {REDUCE, 3}, {NONE, }, {REDUCE, 3}},
  {{NONE, }, {NONE, }, {NONE, }, {NONE, }, {GOTO, 18}, {NONE, }, {NONE, }, {NONE, }, {REDUCE, 5}, {REDUCE, 5}, {REDUCE, 5}, {REDUCE, 5}, {SHIFT_IN, 19}, {NONE, }, {NONE, }, {NONE, }, {SHIFT_IN, 20}, {SHIFT_IN, 21}, {SHIFT_IN, 22}, {REDUCE, 5}, {NONE, }, {NONE, }, {REDUCE, 5}, {REDUCE, 5}, {REDUCE, 5}, {REDUCE, 5}, {REDUCE, 5}, {REDUCE, 5}, {REDUCE, 5}, {NONE, }, {REDUCE, 5}},
  {{NONE, }, {NONE, }, {NONE, }, {NONE, }, {NONE, }, {NONE, }, {NONE, }, {NONE, }, {REDUCE, 6}, {REDUCE, 6}, {REDUCE, 6}, {REDUCE, 6}, {REDUCE, 6}, {NONE, }, {NONE, }, {NONE, }, {REDUCE, 6}, {REDUCE, 6}, {REDUCE, 6}, {REDUCE, 6}, {NONE, }, {NONE, }, {REDUCE, 6}, {REDUCE, 6}, {REDUCE, 6}, {REDUCE, 6}, {REDUCE, 6}, {REDUCE, 6}, {REDUCE, 6}, {NONE, }, {REDUCE, 6}},
  {{NONE, }, {NONE, }, {NONE, }, {NONE, }, {NONE, }, {NONE, }, {NONE, }, {NONE, }, {REDUCE, 7}, {REDUCE, 7}, {REDUCE, 7}, {REDUCE, 7}, {REDUCE, 7}, {NONE, }, {NONE, }, {NONE, }, {REDUCE, 7}, {REDUCE, 7}, {REDUCE, 7}, {REDUCE, 7}, {NONE, }, {NONE, }, {REDUCE, 7}, {REDUCE, 7}, {REDUCE, 7}, {REDUCE, 7}, {REDUCE, 7}, {REDUCE, 7}, {REDUCE, 7}, {NONE, }, {REDUCE, 7}},
  {{GOTO, 23}, {GOTO, 2}, {GOTO, 3}, {GOTO, 4}, {NONE, }, {GOTO, 5}, {NONE, }, {NONE, }, {NONE, }, {SHIFT_IN, 6}, {SHIFT_IN, 7}, {NONE, }, {NONE, }, {NONE, }, {NONE, }, {NONE, }, {NONE, }, {NONE, }, {NONE, }, {SHIFT_IN, 8}, {NONE, }, {NONE, }, {SHIFT_IN, 9}, {SHIFT_IN, 10}, {SHIFT_IN, 11}, {SHIFT_IN, 12}, {SHIFT_IN, 13}, {SHIFT_IN, 14}, {SHIFT_IN, 15}, {NONE, }, {NONE, }},
  {{NONE, }, {NONE, }, {NONE, }, {NONE, }, {NONE, }, {NONE, }, {GOTO, 24}, {GOTO, 25}, {NONE, }, {SHIFT_IN, 26}, {NONE, }, {NONE, }, {NONE, }, {NONE, }, {NONE, }, {NONE, }, {NONE, }, {NONE, }, {NONE, }, {NONE, }, {NONE, }, {SHIFT_IN, 27}, {SHIFT_IN, 28}, {SHIFT_IN, 29}, {SHIFT_IN, 30}, {SHIFT_IN, 31}, {SHIFT_IN, 32}, {SHIFT_IN, 33}, {SHIFT_IN, 34}, {NONE, }, {NONE, }},
  {{NONE, }, {NONE, }, {NONE, }, {NONE, }, {NONE, }, {NONE, }, {NONE, }, {NONE, }, {REDUCE, 17}, {REDUCE, 17}, {REDUCE, 17}, {REDUCE, 17}, {REDUCE, 17}, {NONE, }, {NONE, }, {NONE, }, {REDUCE, 17}, {REDUCE, 17}, {REDUCE, 17}, {REDUCE, 17}, {NONE, }, {NONE, }, {REDUCE, 17}, {REDUCE, 17}, {REDUCE, 17}, {REDUCE, 17}, {REDUCE, 17}, {REDUCE, 17}, {REDUCE, 17}, {NONE, }, {REDUCE, 17}},
  {{NONE, }, {NONE, }, {NONE, }, {NONE, }, {NONE, }, {NONE, }, {NONE, }, {NONE, }, {REDUCE, 18}, {REDUCE, 18}, {REDUCE, 18}, {REDUCE, 18}, {REDUCE, 18}, {NONE, }, {NONE, }, {NONE, }, {REDUCE, 18}, {REDUCE, 18}, {REDUCE, 18}, {REDUCE, 18}, {NONE, }, {NONE, }, {REDUCE, 18}, {REDUCE, 18}, {REDUCE, 18}, {REDUCE, 18}, {REDUCE, 18}, {REDUCE, 18}, {REDUCE, 18}, {NONE, }, {REDUCE, 18}},
  {{NONE, }, {NONE, }, {NONE, }, {NONE, }, {NONE, }, {NONE, }, {NONE, }, {NONE, }, {REDUCE, 19}, {REDUCE, 19}, {REDUCE, 19}, {REDUCE, 19}, {REDUCE, 19}, {NONE, }, {NONE, }, {NONE, }, {REDUCE, 19}, {REDUCE, 19}, {REDUCE, 19}, {REDUCE, 19}, {NONE, }, {NONE, }, {REDUCE, 19}, {REDUCE, 19}, {REDUCE, 19}, {REDUCE, 19}, {REDUCE, 19}, {REDUCE, 19}, {REDUCE, 19}, {NONE, }, {REDUCE, 19}},
  {{NONE, }, {NONE, }, {NONE, }, {NONE, }, {NONE, }, {NONE, }, {NONE, }, {NONE, }, {REDUCE, 20}, {REDUCE, 20}, {REDUCE, 20}, {REDUCE, 20}, {REDUCE, 20}, {NONE, }, {NONE, }, {NONE, }, {REDUCE, 20}, {REDUCE, 20}, {REDUCE, 20}, {REDUCE, 20}, {NONE, }, {NONE, }, {REDUCE, 20}, {REDUCE, 20}, {REDUCE, 20}, {REDUCE, 20}, {REDUCE, 20}, {REDUCE, 20}, {REDUCE, 20}, {NONE, }, {REDUCE, 20}},
  {{NONE, }, {NONE, }, {NONE, }, {NONE, }, {NONE, }, {NONE, }, {NONE, }, {NONE, }, {REDUCE, 21}, {REDUCE, 21}, {REDUCE, 21}, {REDUCE, 21}, {REDUCE, 21}, {NONE, }, {NONE, }, {NONE, }, {REDUCE, 21}, {REDUCE, 21}, {REDUCE, 21}, {REDUCE, 21}, {NONE, }, {NONE, }, {REDUCE, 21}, {REDUCE, 21}, {REDUCE, 21}, {REDUCE, 21}, {REDUCE, 21}, {REDUCE, 21}, {REDUCE, 21}, {NONE, }, {REDUCE, 21}},
  {{NONE, }, {NONE, }, {NONE, }, {NONE, }, {NONE, }, {NONE, }, {NONE, }, {NONE, }, {REDUCE, 22}, {REDUCE, 22}, {REDUCE, 22}, {REDUCE, 22}, {REDUCE, 22}, {NONE, }, {NONE, }, {NONE, }, {REDUCE, 22}, {REDUCE, 22}, {REDUCE, 22}, {REDUCE, 22}, {NONE, }, {NONE, }, {REDUCE, 22}, {REDUCE, 22}, {REDUCE, 22}, {REDUCE, 22}, {REDUCE, 22}, {REDUCE, 22}, {REDUCE, 22}, {NONE, }, {REDUCE, 22}},
  {{NONE, }, {NONE, }, {NONE, }, {NONE, }, {NONE, }, {NONE, }, {NONE, }, {NONE, }, {REDUCE, 23}, {REDUCE, 23}, {REDUCE, 23}, {REDUCE, 23}, {REDUCE, 23}, {NONE, }, {NONE, }, {NONE, }, {REDUCE, 23}, {REDUCE, 23}, {REDUCE, 23}, {REDUCE, 23}, {NONE, }, {NONE, }, {REDUCE, 23}, {REDUCE, 23}, {REDUCE, 23}, {REDUCE, 23}, {REDUCE, 23}, {REDUCE, 23}, {REDUCE, 23}, {NONE, }, {REDUCE, 23}},
  {{NONE, }, {GOTO, 35}, {GOTO, 3}, {GOTO, 4}, {NONE, }, {GOTO, 5}, {NONE, }, {NONE, }, {NONE, }, {SHIFT_IN, 6}, {SHIFT_IN, 7}, {NONE, }, {NONE, }, {NONE, }, {NONE, }, {NONE, }, {NONE, }, {NONE, }, {NONE, }, {SHIFT_IN, 8}, {NONE, }, {NONE, }, {SHIFT_IN, 9}, {SHIFT_IN, 10}, {SHIFT_IN, 11}, {SHIFT_IN, 12}, {SHIFT_IN, 13}, {SHIFT_IN, 14}, {SHIFT_IN, 15}, {NONE, }, {NONE, }},
  {{NONE, }, {NONE, }, {NONE, }, {NONE, }, {NONE, }, {NONE, }, {NONE, }, {NONE, }, {REDUCE, 2}, {REDUCE, 2}, {REDUCE, 2}, {REDUCE, 2}, {NONE, }, {NONE, }, {NONE, }, {NONE, }, {NONE, }, {NONE, }, {NONE, }, {REDUCE, 2}, {NONE, }, {NONE, }, {REDUCE, 2}, {REDUCE, 2}, {REDUCE, 2}, {REDUCE, 2}, {REDUCE, 2}, {REDUCE, 2}, {REDUCE, 2}, {NONE, }, {REDUCE, 2}},
  {{NONE, }, {NONE, }, {NONE, }, {NONE, }, {NONE, }, {NONE, }, {NONE, }, {NONE, }, {REDUCE, 4}, {REDUCE, 4}, {REDUCE, 4}, {REDUCE, 4}, {NONE, }, {NONE, }, {NONE, }, {NONE, }, {NONE, }, {NONE, }, {NONE, }, {REDUCE, 4}, {NONE, }, {NONE, }, {REDUCE, 4}, {REDUCE, 4}, {REDUCE, 4}, {REDUCE, 4}, {REDUCE, 4}, {REDUCE, 4}, {REDUCE, 4}, {NONE, }, {REDUCE, 4}},
  {{NONE, }, {NONE, }, {NONE, }, {NONE, }, {NONE, }, {NONE, }, {NONE, }, {NONE, }, {NONE, }, {NONE, }, {NONE, }, {NONE, }, {NONE, }, {SHIFT_IN, 36}, {NONE, }, {NONE, }, {NONE, }, {NONE, }, {NONE, }, {NONE, }, {NONE, }, {NONE, }, {NONE, }, {NONE, }, {NONE, }, {NONE, }, {NONE, }, {NONE, }, {NONE, }, {NONE, }, {NONE, }},
  {{NONE, }, {NONE, }, {NONE, }, {NONE, }, {NONE, }, {NONE, }, {NONE, }, {NONE, }, {REDUCE, 12}, {REDUCE, 12}, {REDUCE, 12}, {REDUCE, 12}, {NONE, }, {NONE, }, {NONE, }, {NONE, }, {NONE, }, {NONE, }, {NONE, }, {REDUCE, 12}, {NONE, }, {NONE, }, {REDUCE, 12}, {REDUCE, 12}, {REDUCE, 12}, {REDUCE, 12}, {REDUCE, 12}, {REDUCE, 12}, {REDUCE, 12}, {NONE, }, {REDUCE, 12}},
  {{NONE, }, {NONE, }, {NONE, }, {NONE, }, {NONE, }, {NONE, }, {NONE, }, {NONE, }, {REDUCE, 13}, {REDUCE, 13}, {REDUCE, 13}, {REDUCE, 13}, {NONE, }, {NONE, }, {NONE, }, {NONE, }, {NONE, }, {NONE, }, {NONE, }, {REDUCE, 13}, {NONE, }, {NONE, }, {REDUCE, 13}, {REDUCE, 13}, {REDUCE, 13}, {REDUCE, 13}, {REDUCE, 13}, {REDUCE, 13}, {REDUCE, 13}, {NONE, }, {REDUCE, 13}},
  {{NONE, }, {NONE, }, {NONE, }, {NONE, }, {NONE, }, {NONE, }, {NONE, }, {NONE, }, {REDUCE, 14}, {REDUCE, 14}, {REDUCE, 14}, {REDUCE, 14}, {NONE, }, {NONE, }, {NONE, }, {NONE, }, {NONE, }, {NONE, }, {NONE, }, {REDUCE, 14}, {NONE, }, {NONE, }, {REDUCE, 14}, {REDUCE, 14}, {REDUCE, 14}, {REDUCE, 14}, {REDUCE, 14}, {REDUCE, 14}, {REDUCE, 14}, {NONE, }, {REDUCE, 14}},
  {{NONE, }, {NONE, }, {NONE, }, {NONE, }, {NONE, }, {NONE, }, {NONE, }, {NONE, }, {SHIFT_IN, 16}, {NONE, }, {NONE, }, {SHIFT_IN, 37}, {NONE, }, {NONE, }, {NONE, }, {NONE, }, {NONE, }, {NONE, }, {NONE, }, {NONE, }, {NONE, }, {NONE, }, {NONE, }, {NONE, }, {NONE, }, {NONE, }, {NONE, }, {NONE, }, {NONE, }, {NONE, }, {NONE, }},
  {{NONE, }, {NONE, }, {NONE, }, {NONE, }, {NONE, }, {NONE, }, {NONE, }, {GOTO, 38}, {NONE, }, {SHIFT_IN, 26}, {NONE, }, {NONE, }, {NONE, }, {NONE, }, {NONE, }, {NONE, }, {NONE, }, {NONE, }, {NONE, }, {NONE, }, {SHIFT_IN, 39}, {NONE, }, {SHIFT_IN, 28}, {SHIFT_IN, 29}, {SHIFT_IN, 30}, {SHIFT_IN, 31}, {SHIFT_IN, 32}, {SHIFT_IN, 33}, {SHIFT_IN, 34}, {NONE, }, {NONE, }},
  {{NONE, }, {NONE, }, {NONE, }, {NONE, }, {NONE, }, {NONE, }, {NONE, }, {NONE, }, {NONE, }, {REDUCE, 25}, {NONE, }, {NONE, }, {NONE, }, {NONE, }, {NONE, }, {NONE, }, {NONE, }, {NONE, }, {NONE, }, {NONE, }, {REDUCE, 25}, {NONE, }, {REDUCE, 25}, {REDUCE, 25}, {REDUCE, 25}, {REDUCE, 25}, {REDUCE, 25}, {REDUCE, 25}, {REDUCE, 25}, {NONE, }, {NONE, }},
  {{NONE, }, {NONE, }, {NONE, }, {NONE, }, {NONE, }, {NONE, }, {NONE, }, {NONE, }, {NONE, }, {REDUCE, 27}, {NONE, }, {NONE, }, {NONE, }, {NONE, }, {NONE, }, {NONE, }, {NONE, }, {NONE, }, {NONE, }, {NONE, }, {REDUCE, 27}, {NONE, }, {REDUCE, 27}, {REDUCE, 27}, {REDUCE, 27}, {REDUCE, 27}, {REDUCE, 27}, {REDUCE, 27}, {REDUCE, 27}, {SHIFT_IN, 40}, {NONE, }},
  {{NONE, }, {NONE, }, {NONE, }, {NONE, }, {NONE, }, {NONE, }, {GOTO, 41}, {GOTO, 25}, {NONE, }, {SHIFT_IN, 26}, {NONE, }, {NONE, }, {NONE, }, {NONE, }, {NONE, }, {NONE, }, {NONE, }, {NONE, }, {NONE, }, {NONE, }, {NONE, }, {NONE, }, {SHIFT_IN, 28}, {SHIFT_IN, 29}, {SHIFT_IN, 30}, {SHIFT_IN, 31}, {SHIFT_IN, 32}, {SHIFT_IN, 33}, {SHIFT_IN, 34}, {NONE, }, {NONE, }},
  {{NONE, }, {NONE, }, {NONE, }, {NONE, }, {NONE, }, {NONE, }, {NONE, }, {NONE, }, {NONE, }, {REDUCE, 28}, {NONE, }, {NONE, }, {NONE, }, {NONE, }, {NONE, }, {NONE, }, {NONE, }, {NONE, }, {NONE, }, {NONE, }, {REDUCE, 28}, {NONE, }, {REDUCE, 28}, {REDUCE, 28}, {REDUCE, 28}, {REDUCE, 28}, {REDUCE, 28}, {REDUCE, 28}, {REDUCE, 28}, {NONE, }, {NONE, }},
  {{NONE, }, {NONE, }, {NONE, }, {NONE, }, {NONE, }, {NONE, }, {NONE, }, {NONE, }, {NONE, }, {REDUCE, 29}, {NONE, }, {NONE, }, {NONE, }, {NONE, }, {NONE, }, {NONE, }, {NONE, }, {NONE, }, {NONE, }, {NONE, }, {REDUCE, 29}, {NONE, }, {REDUCE, 29}, {REDUCE, 29}, {REDUCE, 29}, {REDUCE, 29}, {REDUCE, 29}, {REDUCE, 29}, {REDUCE, 29}, {NONE, }, {NONE, }},
  {{NONE, }, {NONE, }, {NONE, }, {NONE, }, {NONE, }, {NONE, }, {NONE, }, {NONE, }, {NONE, }, {REDUCE, 30}, {NONE, }, {NONE, }, {NONE, }, {NONE, }, {NONE, }, {NONE, }, {NONE, }, {NONE, }, {NONE, }, {NONE, }, {REDUCE, 30}, {NONE, }, {REDUCE, 30}, {REDUCE, 30}, {REDUCE, 30}, {REDUCE, 30}, {REDUCE, 30}, {REDUCE, 30}, {REDUCE, 30}, {NONE, }, {NONE, }},
  {{NONE, }, {NONE, }, {NONE, }, {NONE, }, {NONE, }, {NONE, }, {NONE, }, {NONE, }, {NONE, }, {REDUCE, 31}, {NONE, }, {NONE, }, {NONE, }, {NONE, }, {NONE, }, {NONE, }, {NONE, }, {NONE, }, {NONE, }, {NONE, }, {REDUCE, 31}, {NONE, }, {REDUCE, 31}, {REDUCE, 31}, {REDUCE, 31}, {REDUCE, 31}, {REDUCE, 31}, {REDUCE, 31}, {REDUCE, 31}, {NONE, }, {NONE, }},
  {{NONE, }, {NONE, }, {NONE, }, {NONE, }, {NONE, }, {NONE, }, {NONE, }, {NONE, }, {NONE, }, {REDUCE, 32}, {NONE, }, {NONE, }, {NONE, }, {NONE, }, {NONE, }, {NONE, }, {NONE, }, {NONE, }, {NONE, }, {NONE, }, {REDUCE, 32}, {NONE, }, {REDUCE, 32}, {REDUCE, 32}, {REDUCE, 32}, {REDUCE, 32}, {REDUCE, 32}, {REDUCE, 32}, {REDUCE, 32}, {NONE, }, {NONE, }},
  {{NONE, }, {NONE, }, {NONE, }, {NONE, }, {NONE, }, {NONE, }, {NONE, }, {NONE, }, {NONE, }, {REDUCE, 33}, {NONE, }, {NONE, }, {NONE, }, {NONE, }, {NONE, }, {NONE, }, {NONE, }, {NONE, }, {NONE, }, {NONE, }, {REDUCE, 33}, {NONE, }, {REDUCE, 33}, {REDUCE, 33}, {REDUCE, 33}, {REDUCE, 33}, {REDUCE, 33}, {REDUCE, 33}, {REDUCE, 33}, {NONE, }, {NONE, }},
  {{NONE, }, {NONE, }, {NONE, }, {NONE, }, {NONE, }, {NONE, }, {NONE, }, {NONE, }, {NONE, }, {REDUCE, 34}, {NONE, }, {NONE, }, {NONE, }, {NONE, }, {NONE, }, {NONE, }, {NONE, }, {NONE, }, {NONE, }, {NONE, }, {REDUCE, 34}, {NONE, }, {REDUCE, 34}, {REDUCE, 34}, {REDUCE, 34}, {REDUCE, 34}, {REDUCE, 34}, {REDUCE, 34}, {REDUCE, 34}, {NONE, }, {NONE, }},
  {{NONE, }, {NONE, }, {GOTO, 17}, {GOTO, 4}, {NONE, }, {GOTO, 5}, {NONE, }, {NONE, }, {REDUCE, 0}, {SHIFT_IN, 6}, {SHIFT_IN, 7}, {REDUCE, 0}, {NONE, }, {NONE, }, {NONE, }, {NONE, }, {NONE, }, {NONE, }, {NONE, }, {SHIFT_IN, 8}, {NONE, }, {NONE, }, {SHIFT_IN, 9}, {SHIFT_IN, 10}, {SHIFT_IN, 11}, {SHIFT_IN, 12}, {SHIFT_IN, 13}, {SHIFT_IN, 14}, {SHIFT_IN, 15}, {NONE, }, {REDUCE, 0}},
  {{NONE, }, {NONE, }, {NONE, }, {NONE, }, {NONE, }, {NONE, }, {NONE, }, {NONE, }, {NONE, }, {NONE, }, {NONE, }, {NONE, }, {NONE, }, {NONE, }, {SHIFT_IN, 42}, {SHIFT_IN, 43}, {NONE, }, {NONE, }, {NONE, }, {NONE, }, {NONE, }, {NONE, }, {NONE, }, {NONE, }, {NONE, }, {NONE, }, {NONE, }, {NONE, }, {NONE, }, {NONE, }, {NONE, }},
  {{NONE, }, {NONE, }, {NONE, }, {NONE, }, {NONE, }, {NONE, }, {NONE, }, {NONE, }, {REDUCE, 8}, {REDUCE, 8}, {REDUCE, 8}, {REDUCE, 8}, {REDUCE, 8}, {NONE, }, {NONE, }, {NONE, }, {REDUCE, 8}, {REDUCE, 8}, {REDUCE, 8}, {REDUCE, 8}, {NONE, }, {NONE, }, {REDUCE, 8}, {REDUCE, 8}, {REDUCE, 8}, {REDUCE, 8}, {REDUCE, 8}, {REDUCE, 8}, {REDUCE, 8}, {NONE, }, {REDUCE, 8}},
  {{NONE, }, {NONE, }, {NONE, }, {NONE, }, {NONE, }, {NONE, }, {NONE, }, {NONE, }, {NONE, }, {REDUCE, 24}, {NONE, }, {NONE, }, {NONE, }, {NONE, }, {NONE, }, {NONE, }, {NONE, }, {NONE, }, {NONE, }, {NONE, }, {REDUCE, 24}, {NONE, }, {REDUCE, 24}, {REDUCE, 24}, {REDUCE, 24}, {REDUCE, 24}, {REDUCE, 24}, {REDUCE, 24}, {REDUCE, 24}, {NONE, }, {NONE, }},
  {{NONE, }, {NONE, }, {NONE, }, {NONE, }, {NONE, }, {NONE, }, {NONE, }, {NONE, }, {REDUCE, 15}, {REDUCE, 15}, {REDUCE, 15}, {REDUCE, 15}, {REDUCE, 15}, {NONE, }, {NONE, }, {NONE, }, {REDUCE, 15}, {REDUCE, 15}, {REDUCE, 15}, {REDUCE, 15}, {NONE, }, {NONE, }, {REDUCE, 15}, {REDUCE, 15}, {REDUCE, 15}, {REDUCE, 15}, {REDUCE, 15}, {REDUCE, 15}, {REDUCE, 15}, {NONE, }, {REDUCE, 15}},
  {{NONE, }, {NONE, }, {NONE, }, {NONE, }, {NONE, }, {NONE, }, {NONE, }, {NONE, }, {NONE, }, {SHIFT_IN, 44}, {NONE, }, {NONE, }, {NONE, }, {NONE, }, {NONE, }, {NONE, }, {NONE, }, {NONE, }, {NONE, }, {NONE, }, {NONE, }, {NONE, }, {NONE, }, {NONE, }, {NONE, }, {NONE, }, {NONE, }, {NONE, }, {NONE, }, {NONE, }, {NONE, }},
  {{NONE, }, {NONE, }, {NONE, }, {NONE, }, {NONE, }, {NONE, }, {NONE, }, {GOTO, 38}, {NONE, }, {SHIFT_IN, 26}, {NONE, }, {NONE, }, {NONE, }, {NONE, }, {NONE, }, {NONE, }, {NONE, }, {NONE, }, {NONE, }, {NONE, }, {SHIFT_IN, 45}, {NONE, }, {SHIFT_IN, 28}, {SHIFT_IN, 29}, {SHIFT_IN, 30}, {SHIFT_IN, 31}, {SHIFT_IN, 32}, {SHIFT_IN, 33}, {SHIFT_IN, 34}, {NONE, }, {NONE, }},
  {{NONE, }, {NONE, }, {NONE, }, {NONE, }, {NONE, }, {NONE, }, {NONE, }, {NONE, }, {REDUCE, 9}, {REDUCE, 9}, {REDUCE, 9}, {REDUCE, 9}, {NONE, }, {NONE, }, {NONE, }, {NONE, }, {NONE, }, {NONE, }, {NONE, }, {REDUCE, 9}, {NONE, }, {NONE, }, {REDUCE, 9}, {REDUCE, 9}, {REDUCE, 9}, {REDUCE, 9}, {REDUCE, 9}, {REDUCE, 9}, {REDUCE, 9}, {NONE, }, {REDUCE, 9}},
  {{NONE, }, {NONE, }, {NONE, }, {NONE, }, {NONE, }, {NONE, }, {NONE, }, {NONE, }, {NONE, }, {NONE, }, {NONE, }, {NONE, }, {NONE, }, {SHIFT_IN, 46}, {SHIFT_IN, 47}, {NONE, }, {NONE, }, {NONE, }, {NONE, }, {NONE, }, {NONE, }, {NONE, }, {NONE, }, {NONE, }, {NONE, }, {NONE, }, {NONE, }, {NONE, }, {NONE, }, {NONE, }, {NONE, }},
  {{NONE, }, {NONE, }, {NONE, }, {NONE, }, {NONE, }, {NONE, }, {NONE, }, {NONE, }, {NONE, }, {REDUCE, 26}, {NONE, }, {NONE, }, {NONE, }, {NONE, }, {NONE, }, {NONE, }, {NONE, }, {NONE, }, {NONE, }, {NONE, }, {REDUCE, 26}, {NONE, }, {REDUCE, 26}, {REDUCE, 26}, {REDUCE, 26}, {REDUCE, 26}, {REDUCE, 26}, {REDUCE, 26}, {REDUCE, 26}, {NONE, }, {NONE, }},
  {{NONE, }, {NONE, }, {NONE, }, {NONE, }, {NONE, }, {NONE, }, {NONE, }, {NONE, }, {REDUCE, 16}, {REDUCE, 16}, {REDUCE, 16}, {REDUCE, 16}, {REDUCE, 16}, {NONE, }, {NONE, }, {NONE, }, {REDUCE, 16}, {REDUCE, 16}, {REDUCE, 16}, {REDUCE, 16}, {NONE, }, {NONE, }, {REDUCE, 16}, {REDUCE, 16}, {REDUCE, 16}, {REDUCE, 16}, {REDUCE, 16}, {REDUCE, 16}, {REDUCE, 16}, {NONE, }, {REDUCE, 16}},
  {{NONE, }, {NONE, }, {NONE, }, {NONE, }, {NONE, }, {NONE, }, {NONE, }, {NONE, }, {NONE, }, {NONE, }, {NONE, }, {NONE, }, {NONE, }, {NONE, }, {SHIFT_IN, 48}, {NONE, }, {NONE, }, {NONE, }, {NONE, }, {NONE, }, {NONE, }, {NONE, }, {NONE, }, {NONE, }, {NONE, }, {NONE, }, {NONE, }, {NONE, }, {NONE, }, {NONE, }, {NONE, }},
  {{NONE, }, {NONE, }, {NONE, }, {NONE, }, {NONE, }, {NONE, }, {NONE, }, {NONE, }, {REDUCE, 10}, {REDUCE, 10}, {REDUCE, 10}, {REDUCE, 10}, {NONE, }, {NONE, }, {NONE, }, {NONE, }, {NONE, }, {NONE, }, {NONE, }, {REDUCE, 10}, {NONE, }, {NONE, }, {REDUCE, 10}, {REDUCE, 10}, {REDUCE, 10}, {REDUCE, 10}, {REDUCE, 10}, {REDUCE, 10}, {REDUCE, 10}, {NONE, }, {REDUCE, 10}},
  {{NONE, }, {NONE, }, {NONE, }, {NONE, }, {NONE, }, {NONE, }, {NONE, }, {NONE, }, {REDUCE, 11}, {REDUCE, 11}, {REDUCE, 11}, {REDUCE, 11}, {NONE, }, {NONE, }, {NONE, }, {NONE, }, {NONE, }, {NONE, }, {NONE, }, {REDUCE, 11}, {NONE, }, {NONE, }, {REDUCE, 11}, {REDUCE, 11}, {REDUCE, 11}, {REDUCE, 11}, {REDUCE, 11}, {REDUCE, 11}, {REDUCE, 11}, {NONE, }, {REDUCE, 11}} };

const Production productions[35] = 
{{REG, 3},  {REG, 1},  {TERM, 2},  {TERM, 1},  {FACTOR, 2}, 
 {FACTOR, 1},  {ATOM, 1},  {ATOM, 1},  {ATOM, 3},  {REPEAT, 3}, 
 {REPEAT, 4},  {REPEAT, 5},  {REPEAT, 1},  {REPEAT, 1},  {REPEAT, 1}, 
 {LETTER_CLASS, 3},  {LETTER_CLASS, 4},  {LETTER_CLASS, 1},  {LETTER_CLASS, 1},  {LETTER_CLASS, 1}, 
 {LETTER_CLASS, 1},  {LETTER_CLASS, 1},  {LETTER_CLASS, 1},  {LETTER_CLASS, 1},  {LETTER_SET, 2}, 
 {LETTER_SET, 1},  {SET_FACTOR, 3},  {SET_FACTOR, 1},  {SET_FACTOR, 1},  {SET_FACTOR, 1}, 
 {SET_FACTOR, 1},  {SET_FACTOR, 1},  {SET_FACTOR, 1},  {SET_FACTOR, 1},  {SET_FACTOR, 1}};

/*
该自动机生成算法可以用于正则表达式引擎，用来生成
判断某个串是否匹配某个模式的自动机，但不能用来生
成词法分析器的自动机。因为词法分析器要求能区分多
个模式，而该算法生成的自动机内可能包含某个同时接
受两个对应语言并不相交的模式的状态，因此若该状态
接受了某个串，将不能判断该串属于哪个模式。
*/
void main()
{
	readModeStr();
	
	if (!processPrimitiveStr(mode, REG_EXPR))
		goto L;

	printProcessedStr(mode);
	
	if (!parse(mode))
		goto L;

	printAbstractSyntaxTree(tree);
	printf("\n");
	formFollowPosBy(tree);
	generateDFA();
	reduceDFA();
	runDFA();
L:
	resume();
}

/*
把文件中的模式串读入内存
*/
void readModeStr()
{
	char *fileName = "C:\\Users\\lenovo\\Desktop\\mode.txt";
	fp = fopen(fileName, "r");
	
	if (fp) {
		unsigned short i = 0;
		while (true) {
			mode[i] = fgetc(fp);
			if (mode[i] == EOF ||
				mode[i] == '\n') {
				mode[i] = '\0';
				break;
			}
			++i;
		}
		printf("模式串读取成功\n");
		fclose(fp);
	} else {
		printf("模式串读取失败\n");
		exit(1);
	}
}

/*
处理原始字符串str。若strType为GENERAL，则str被
视为一般字符串，该过程会把str中的转义字符转换为
它想表示的字符；若strType为REG_EXPR，则str被视
为扩展正则表达式，除了上述处理外，还将处理元字
符的转义、构造和repeat单元的构造
*/
bool processPrimitiveStr(char *str, StringType strType)
{
	char c, *temp;

	while (*str) {
		if (str[0] == '\\') {
			if (str[1] >= '0' &&
				str[1] <= '7') {
				temp = str;
				c = charByOctal(&str);
				if (!c && strType == GENERAL) {
					str[0] = '\0';
					return true;
				}
				if (c >= 0 && c < 128) {
					*temp = c ? c : extendChar(NULL_CHAR);
					while (++temp < str)
						*temp = extendChar(0);
				} else {
					printf("八进制转义字符超出了ASCII字符的范围0~127\n");
					return false;
				}
			} else if (str[1] == 'x') {
				if (isHexadecimalDigit(str[2])) {
					temp = str;
					c = charByHexadecimal(&str);
					if (!c && strType == GENERAL) {
						str[0] = '\0';
						return true;
					}
					if (c >= 0 && c < 128) {
						*temp = c ? c : extendChar(NULL_CHAR);
						while (++temp < str)
							*temp = extendChar(0);
					} else {
						printf("十六进制转义字符超出了ASCII字符的范围0~127\n");
						return false;
					}
				} else {
					printf("非法十六进制转义字符\n");
					return false;
				}
			} else {
				switch (str[1]) {
					case 'a':
						str[0] = '\a';
						break;
					case 'b':
						str[0] = '\b';
						break;
					case 'f':
						str[0] = '\f';
						break;
					case 'n':
						str[0] = '\n';
						break;
					case 'r':
						str[0] = '\r';
						break;
					case 't':
						str[0] = '\t';
						break;
					case 'v':
						str[0] = '\v';
						break;
					case '\\':
						break;
					case '|':
					case '*':
					case '(':
					case ')':
					case '+':
					case '?':
					case '.':
					case '[':
					case ']':
					case '-':
					case '^':
					case '{':
					case '}':
					case ',':
						if (strType == GENERAL) {
							printf("非法转义字符\n");
							return false;
						}
						str[0] = str[1];
						break;
					case 'd':
					case 'D':
					case 's':
					case 'S':
					case 'w':
					case 'W':
						if (strType == GENERAL) {
							printf("非法转义字符\n");
							return false;
						}
						switch (str[1]) {
							case 'd':
								str[0] = extendChar(DIGIT);
								break;
							case 'D':
								str[0] = extendChar(NON_DIGIT);
								break;
							case 's':
								str[0] = extendChar(SPACE);
								break;
							case 'S':
								str[0] = extendChar(NON_SPACE);
								break;
							case 'w':
								str[0] = extendChar(WORD);
								break;
							case 'W':
								str[0] = extendChar(NON_WORD);
						}
						break;
					default:
						printf("非法转义字符\n");
						return false;
				}
				str[1] = extendChar(0);
				str += 2;
			}
		} else if (strType == REG_EXPR) {
			switch (str[0]) {
				case '|':
					str[0] = extendChar(OR);
					break;
				case '*':
					str[0] = extendChar(STAR);
					break;
				case '(':
					str[0] = extendChar(LEFT_PAR);
					break;
				case ')':
					str[0] = extendChar(RIGHT_PAR);
					break;
				case '+':
					str[0] = extendChar(PLUS);
					break;
				case '?':
					str[0] = extendChar(QUESTION_MARK);
					break;
				case '.':
					str[0] = extendChar(DOT);
					break;
				case '[':
					str[0] = extendChar(LEFT_BRACKET);
					break;
				case ']':
					str[0] = extendChar(RIGHT_BRACKET);
					break;
				case '-':
					str[0] = extendChar(EN_DASH);
					break;
				case '^':
					if (*(str - 1) == extendChar(LEFT_BRACKET))
						str[0] = extendChar(CARET);
					break;
				case '{':
					str[0] = extendChar(LEFT_BRACE);
					if (!constructRepeatUnit(&str)) {
						printf("repeat单元语法错误\n");
						return false;
					}
					continue;
				case '}':
					str[0] = extendChar(RIGHT_BRACE);
			}
			++str;
		} else
			++str;
	}

	if (strType == REG_EXPR) {
		str[0] = extendChar(END);
		str[1] = '\0';
	}

	printf("原始字符串处理完成\n");
	return true;
}

/*
以*pstr为起始地址开始向后扫描，完成一个repeat单
元的构建，若构建完成，返回true，否则返回false
*/
bool constructRepeatUnit(char **pstr)
{
	char *head = ++*pstr, *end;
	unsigned short base = 1;
	unsigned short num1 = 0, num2 = 0;

	if (*head < '0' || *head > '9')
		return false;

	while (**pstr >= '0' && **pstr <= '9')
		++*pstr;

	if (**pstr != ',' && **pstr != '}')
		return false;

	end = *pstr - 1;

	while (end >= head) {
		num1 += (*end - '0') * base;
		base *= 10;
		*end-- = extendChar(0);
	}

	*head = extendChar(NUM);

	if (**pstr == '}' || **pstr == ',' && *(*pstr + 1) == '}') {
		if (**pstr == '}')
			*(*pstr)++ = extendChar(RIGHT_BRACE);
		else {
			*(*pstr)++ = extendChar(COMMA);
			*(*pstr)++ = extendChar(RIGHT_BRACE);
		}
		if (!numList)
			numList = newList();
		addElem(numList, newShort(num1));
		return true;
	}

	*(*pstr)++ = extendChar(COMMA);

	if (**pstr < '0' || **pstr > '9')
		return false;

	head = *pstr;

	while (**pstr >= '0' && **pstr <= '9')
		++*pstr;

	if (**pstr != '}')
		return false;

	end = *pstr - 1;
	base = 1;

	while (end >= head) {
		num2 += (*end - '0') * base;
		base *= 10;
		*end-- = extendChar(0);
	}

	if (num1 >= num2)
		return false;

	*head = extendChar(NUM);
	*(*pstr)++ = extendChar(RIGHT_BRACE);
	
	if (!numList)
		numList = newList();

	addElem(numList, newShort(num1));
	addElem(numList, newShort(num2));

	return true;
}

/*
打印经过转义处理或（和）构造处理的字符串
*/
void printProcessedStr(char *source)
{
	char c;

	while (c = *source++)
		switch (charType(c)) {
			case LETTER:
				printf("%c", c);
				break;
			case LEFT_PAR:
				printf("(");
				break;
			case RIGHT_PAR:
				printf(")");
				break;
			case STAR:
				printf("*");
				break;
			case OR:
				printf("|");
				break;
			case NULL_CHAR:
				printf("\\0");
				break;
			case PLUS:
				printf("+");
				break;
			case QUESTION_MARK:
				printf("?");
				break;
			case DOT:
				printf(".");
				break;
			case LEFT_BRACKET:
				printf("[");
				break;
			case RIGHT_BRACKET:
				printf("]");
				break;
			case EN_DASH:
				printf("-");
				break;
			case CARET:
				printf("^");
				break;
			case LEFT_BRACE:
				printf("{");
				break;
			case RIGHT_BRACE:
				printf("}");
				break;
			case NUM:
				printf("num");
				break;
			case COMMA:
				printf(",");
				break;
			case DIGIT:
				printf("\\d");
				break;
			case NON_DIGIT:
				printf("\\D");
				break;
			case SPACE:
				printf("\\s");
				break;
			case NON_SPACE:
				printf("\\S");
				break;
			case WORD:
				printf("\\w");
				break;
			case NON_WORD:
				printf("\\W");
				break;
			case END:
				printf("$");
		}

	printf("\n");
}

/*
将八进制转义字符转换为它所代表的字符并返回
*/
char charByOctal(char **mode)
{
	unsigned short i = 0, base = 1, result = 0;
	char *start = *mode + 1, *end = start;

	while (i < 3 && *end >= '0' && *end <= '7') {
		++end;
		++i;
	}

	*mode = end;

	while (end > start) {
		result += (*(end - 1) - '0') * base;
		base <<= 3;
		--end;
	}

	if (result < 128)
		return (char)result;
	else
		return extendChar(0);
}

/*
将十六进制转义字符转换为它所代表的字符并返回
*/
char charByHexadecimal(char **mode)
{
	unsigned short i = 0, base = 1, result = 0;
	char *start = *mode + 2, *end = start;

	while (i < 2 && isHexadecimalDigit(*end)) {
		++end;
		++i;
	}

	*mode = end;

	while (end > start) {
		if (*(end - 1) >= 'A' && *(end - 1) <= 'F')
			result += (*(end - 1) - 'A') * base;
		else if (*(end - 1) >= 'a' && *(end - 1) <= 'f')
			result += (*(end - 1) - 'a') * base;
		else
			result += (*(end - 1) - '0') * base;
		base <<= 4;
		--end;
	}

	if (result < 128)
		return (char)result;
	else
		return extendChar(0);
}

/*
构造可以表示type的字符并返回
*/
char extendChar(int type)
{
	char c = type;
	c |= 0x80;
	return c;
}

/*
返回扩展字符的类型
*/
int charType(char c)
{
	if (c >= 0 && c < 128)
		return LETTER;
	else {
		c &= 0x7f;
		return c;
	}
}

/*
把字符c加入字母集中，若已存在则忽略
*/
void addToLetterList(char c)
{
	Char *ch = newChar(c);

	if (!letterList)
		letterList = newList();

	if (!contains(letterList, ch))
		addElem(letterList, ch);
	else
		free(ch);
}

/*
判断字符c是否十六进制的数位字符
*/
bool isHexadecimalDigit(char c)
{
	return (c >= 'A' && c <= 'F' ||
			c >= 'a' && c <= 'f' ||
			c >= '0' && c <= '9');
}

/*
根据语法分析表对模式串进行语法分析，
并在此过程中生成模式串的抽象语法树
*/
bool parse(char *source)
{
	stack = newStack();
	push(stack, newRecord(0, NULL));

	while (*source) {
		unsigned short terminal = charType(*source);
		unsigned short state, id;
		ActionType actionType;
		Record *record;
		if (!terminal) {
			++source;
			continue;
		}
		if (terminal == NULL_CHAR)
			terminal = LETTER;
		state = ((Record*)stackElem(stack, 0))->state;
		actionType = parsingTable[state][terminal].actionType;
		id = parsingTable[state][terminal].id;
		switch (actionType) {
			case SHIFT_IN:
				record = newRecord(id, NULL);
				switch (terminal) {
					case LETTER:
						record->attr = newChar(*source);
						break;
					case NUM:
						record->attr = nthElem(numList, ++numCount);
						break;
					case DOT:
					{
						Char *ch = newChar('\n'), *ch1;
						List *list = charRange(1, 127);
						ch1 = equalElem(list, ch);
						containerOnly = false;
						removeElem(list, ch1);
						free(ch);
						containerOnly = true;
						record->attr = list;
						break;
					}
					case DIGIT:
						record->attr = charRange('0', '9');
						break;
					case NON_DIGIT:
					{
						List *list = charRange(1, '0' - 1);
						List *list1 = charRange('9' + 1, 127);
						addList(list, list1, REPEAT_ALLOWED);
						containerOnly = true;
						freeList(list1);
						record->attr = list;
						break;
					}
					case SPACE:
					{
						List *list = newList();
						addElem(list, newChar(' '));
						addElem(list, newChar('\f'));
						addElem(list, newChar('\n'));
						addElem(list, newChar('\r'));
						addElem(list, newChar('\t'));
						addElem(list, newChar('\v'));
						record->attr = list;
						break;
					}
					case NON_SPACE:
					{
						List *list = charRange(1, 127);
						Char *s = newChar(' ');
						Char *f = newChar('\f');
						Char *n = newChar('\n');
						Char *r = newChar('\r');
						Char *t = newChar('\t');
						Char *v = newChar('\v');
						containerOnly = false;
						removeElem(list, s);
						removeElem(list, f);
						removeElem(list, n);
						removeElem(list, r);
						removeElem(list, t);
						removeElem(list, v);
						containerOnly = true;
						free(s);
						free(f);
						free(n);
						free(r);
						free(t);
						free(v);
						record->attr = list;
						break;
					}
					case WORD:
					{
						List *list = charRange('0', '9');
						List *list1 = charRange('A', 'Z');
						List *list2 = charRange('a', 'z');
						addList(list, list1, REPEAT_ALLOWED);
						addList(list, list2, REPEAT_ALLOWED);
						addElem(list, newChar('_'));
						containerOnly = true;
						freeList(list1);
						freeList(list2);
						record->attr = list;
						break;
					}
					case NON_WORD:
					{
						List *list = charRange(1, '0' - 1);
						List *list1 = charRange('9' + 1, 'A' - 1);
						List *list2 = charRange('Z' + 1, 'a' - 1);
						List *list3 = charRange('z' + 1, 127);
						Char *ch = newChar('_');
						addList(list, list1, REPEAT_ALLOWED);
						addList(list, list2, REPEAT_ALLOWED);
						addList(list, list3, REPEAT_ALLOWED);
						containerOnly = false;
						removeElem(list, ch);
						free(ch);
						containerOnly = true;
						freeList(list1);
						freeList(list2);
						freeList(list3);
						record->attr = list;
					}
				}
				push(stack, record);
				++source;
				break;
			case REDUCE:
				if (!reduce(id))
					return false;
				break;
			case ACCEPT:
				tree = newTreeNode(extendChar(CAT));
				tree->left = ((Record*)pop(stack))->attr;
				tree->right = newTreeNode(extendChar(AUX));
				printf("模式串语法分析完成\n");
				return true;
			default:
				printf("模式串语法错误\n");
				return false;
		}
	}

	return false;
}

/*
根据序号为id的产生式对语法分析栈进行归约，并执行相应的语义动作
*/
bool reduce(unsigned short id)
{
	unsigned short head = productions[id].head;
	unsigned short bodySize = productions[id].bodySize;
	unsigned short state, gotoState;
	void *attr = NULL;

	switch (id) {
		case 0:
			attr = ((Record*)stackElem(stack, 2))->attr;
			attr = insert(attr, ((Record*)stackElem(stack, 0))->attr, OR);
			break;
		case 2:
			attr = ((Record*)stackElem(stack, 1))->attr;
			attr = insert(attr, ((Record*)stackElem(stack, 0))->attr, CAT);
			break;
		case 4:
		{
			unsigned short i, orCount;
			Repeat *repeat = ((Record*)stackElem(stack, 0))->attr;
			TreeNode *node = ((Record*)stackElem(stack, 1))->attr;
			switch (repeat->mode) {
				case ABSENT:
					if (!repeat->num1)
						attr = newTreeNode(extendChar(NULL_CHAR));
					else
						for (i = 1; i <= repeat->num1; ++i)
							attr = insert(attr, cloneTree(node), CAT);
					break;
				case FINITE:
				{
					TreeNode *orNode = cloneTree(node);
					orNode = insert(orNode, newTreeNode(extendChar(NULL_CHAR)), OR);
					for (i = 1; i <= repeat->num1; ++i)
						attr = insert(attr, cloneTree(node), CAT);
					orCount = repeat->num2 - repeat->num1;
					for (i = 1; i <= orCount; ++i)
						attr = insert(attr, cloneTree(orNode), CAT);
					freeTree(orNode);
					break;
				}
				case INFINITE:
				{
					TreeNode *starNode = newTreeNode(extendChar(STAR));
					starNode->left = cloneTree(node);
					for (i = 1; i <= repeat->num1; ++i)
						attr = insert(attr, cloneTree(node), CAT);
					attr = insert(attr, starNode, CAT);
				}
			}
			free(repeat);
			freeTree(node);
			break;
		}
		case 7:
		{
			Char *ch = ((Record*)stackElem(stack, 0))->attr;
			attr = newTreeNode(ch->val);
			free(ch);
			break;
		}
		case 8:
			attr = ((Record*)stackElem(stack, 1))->attr;
			break;
		case 9:
		{
			Short *sh = ((Record*)stackElem(stack, 1))->attr;
			attr = newRepeat(ABSENT, sh->val, 0);
			break;
		}
		case 10:
		{
			Short *sh = ((Record*)stackElem(stack, 2))->attr;
			attr = newRepeat(INFINITE, sh->val, 0);
			break;
		}
		case 11:
		{
			Short *sh1 = ((Record*)stackElem(stack, 3))->attr;
			Short *sh2 = ((Record*)stackElem(stack, 1))->attr;
			attr = newRepeat(FINITE, sh1->val, sh2->val);
			break;
		}
		case 12:
			attr = newRepeat(FINITE, 0, 1);
			break;
		case 13:
			attr = newRepeat(INFINITE, 1, 0);
			break;
		case 14:
			attr = newRepeat(INFINITE, 0, 0);
			break;
		case 15:
		case 16:
		case 17:
		case 18:
		case 19:
		case 20:
		case 21:
		case 22:
		case 23:
		{
			List *charList;
			if (id == 16) {
				char c;
				List *charList1 = ((Record*)stackElem(stack, 1))->attr;
				charList = newList();
				for (c = 1; c < 128; ++c) {
					Char *ch = newChar(c);
					char c1 = c;
					c1 &= 0x80;
					if (c1) {
						free(ch);
						break;
					}
					if (!contains(charList1, ch))
						addElem(charList, ch);
					else
						free(ch);
				}
				containerOnly = false;
				freeList(charList1);
			} else if (id == 15)
				charList = ((Record*)stackElem(stack, 1))->attr;
			else
				charList = ((Record*)stackElem(stack, 0))->attr;
			if (!charList->head) {
				free(charList);
				attr = newTreeNode(extendChar(NULL_CHAR));
			} else {
				Node *walk;
				for (walk = charList->head; walk; walk = walk->next) {
					Char *ch = walk->elem;
					attr = insert(attr, newTreeNode(ch->val), OR);
				}
				containerOnly = false;
				freeList(charList);
			}
			containerOnly = true;
			break;
		}
		case 24:
		{
			List *list = ((Record*)stackElem(stack, 1))->attr;
			List *list1 = ((Record*)stackElem(stack, 0))->attr;
			addList(list, list1, REPEAT_BANNED);
			attr = list;
			containerOnly = true;
			freeList(list1);
			break;
		}
		case 26:
		{
			Char *ch1 = ((Record*)stackElem(stack, 2))->attr;
			Char *ch2 = ((Record*)stackElem(stack, 0))->attr;
			if (charType(ch1->val) == NULL_CHAR
				|| charType(ch2->val) == NULL_CHAR) {
				printf("字符集中不允许出现空字符\n");
				return false;
			}
			attr = charRange(ch1->val, ch2->val);
			if (!attr) {
				printf("字符集格式有误\n");
				return false;
			}
			free(ch1);
			free(ch2);
			break;
		}
		case 27:
		{
			Char *ch = ((Record*)stackElem(stack, 0))->attr;
			if (charType(ch->val) == NULL_CHAR) {
				printf("字符集中不允许出现空字符\n");
				return false;
			}
			attr = newList();
			addElem(attr, ch);
			break;
		}
		case 1:
		case 3:
		case 5:
		case 6:
		case 25:
		case 28:
		case 29:
		case 30:
		case 31:
		case 32:
		case 33:
		case 34:
			attr = ((Record*)stackElem(stack, 0))->attr;
	}

	while (bodySize--)
		free(pop(stack));
	
	state = ((Record*)stackElem(stack, 0))->state;
	gotoState = parsingTable[state][head].id;
	push(stack, newRecord(gotoState, attr));

	return true;
}

/*
该算法的目标是，在构造抽象语法树的子树的过程中，
构造出尽最大可能平衡的OR子树或CATENATE子树，子
树的类型由参数type指定
*/
TreeNode *insert(TreeNode *tree, TreeNode *node, int type)
{
	if (!node)
		return tree;

	if (!tree)
		return node;

	if (charType(tree->val) != type ||
		sonCount(tree->left, type) == sonCount(tree->right, type)) {
		TreeNode *newNode = newTreeNode(extendChar(type));
		newNode->left = tree;
		newNode->right = node;
		return newNode;
	} else {
		tree->right = insert(tree->right, node, type);
		return tree;
	}
}

/*
返回树tree中不是type类型子树的棵数（若tree本身不是
type类型的，则返回1，即只考虑tree本身，不再考虑tree
的左右子树
*/
unsigned short sonCount(TreeNode *tree, int type)
{
	if (!tree)
		return 0;

	if (charType(tree->val) != type)
		return 1;

	return sonCount(tree->left, type)
		+ sonCount(tree->right, type);
}

/*
返回树tree的克隆体
*/
TreeNode *cloneTree(TreeNode *node)
{
	TreeNode *clone;

	if (!node)
		return NULL;

	clone = newTreeNode(node->val);
	clone->left = cloneTree(node->left);
	clone->right = cloneTree(node->right);
	
	return clone;
}

/*
返回闭区间[start, end]内的字符集合
*/
List *charRange(char start, char end)
{
	char c;
	List *list;

	if (start >= end)
		return NULL;

	list = newList();

	while (start <= end) {
		addElem(list, newChar(start++));
		c = start;
		c &= 0x80;
		if (c)
			break;
	}

	return list;
}

/*
打印抽象语法树
*/
void printAbstractSyntaxTree(TreeNode *node)
{
	bool parenthesis;
	int type = charType(node->val);

	switch (type) {
		case LETTER:
			printf("%c", node->val);
			break;
		case NULL_CHAR:
			printf("\\0");
			break;
		case AUX:
			printf("$");
			break;
		case STAR:
			type = charType(node->left->val);
			parenthesis = type == OR || type == CAT;
			if (parenthesis)
				printf("(");
			printAbstractSyntaxTree(node->left);
			if (parenthesis)
				printf(")");
			printf("*");
			break;
		case CAT:
			parenthesis = charType(node->left->val) == OR;
			if (parenthesis)
				printf("(");
			printAbstractSyntaxTree(node->left);
			if (parenthesis)
				printf(")");
			parenthesis = charType(node->right->val) == OR;
			if (parenthesis)
				printf("(");
			printAbstractSyntaxTree(node->right);
			if (parenthesis)
				printf(")");
			break;
		case OR:
			printAbstractSyntaxTree(node->left);
			printf("|");
			printAbstractSyntaxTree(node->right);
	}
}

/*
计算结点node的nullable函数
*/
bool nullable(TreeNode *node)
{
	switch (charType(node->val)) {
		case NULL_CHAR:
		case STAR:
			return true;
		case LETTER:
		case AUX:
			return false;
		case CAT:
			return nullable(node->left) && nullable(node->right);
		case OR:
			return nullable(node->left) || nullable(node->right);
		default:
			return false;
	}
}

/*
计算结点node的POSITION函数，
type为FIRST时计算firstpos，
type为LAST时计算lastpos
*/
List *POSITION(TreeNode *node, PositionType type)
{
	List *list = newList();

	if (!node) {
		free(list);
		return NULL;
	}

	switch (charType(node->val)) {
		case LETTER:
			addElem(list, node);
			return list;
		case AUX:
			addElem(list, node);
			return list;
		case STAR:
			free(list);
			return POSITION(node->left, type);
		case CAT:
			if (type == FIRST ? nullable(node->left) : nullable(node->right)) {
				List *list1 = POSITION(type == FIRST ? node->left : node->right, type);
				List *list2 = POSITION(type == FIRST ? node->right : node->left, type);
				if (list1) {
					free(list);
					list = list1;
				}
				addList(list, list2, REPEAT_BANNED);
				if (!list->head) {
					free(list);
					return NULL;
				}
				return list;
			} else {
				free(list);
				return POSITION(type == FIRST ? node->left : node->right, type);
			}
		case OR:
		{
			List *list1 = POSITION(node->left, type);
			List *list2 = POSITION(node->right, type);
			if (list1) {
				free(list);
				list = list1;
			}
			addList(list, list2, REPEAT_BANNED);
			if (!list->head) {
				free(list);
				return NULL;
			}
			return list;
		}
		default:
			return NULL;
	}
}

/*
计算出能够根据结点node计算的抽象语法树的followpos集合
*/
void formFollowPosBy(TreeNode *node)
{
	int type;

	if (!node)
		return;

	type = charType(node->val);

	if (type == STAR || type == CAT) {

		List *first, *last;
		Node *walk;

		if (type == STAR) {
			first = POSITION(node, FIRST);
			last = POSITION(node, LAST);
		} else {
			first = POSITION(node->right, FIRST);
			last = POSITION(node->left, LAST);
		}

		if (first && last)
			for (walk = last->head; walk; walk = walk->next) {
				TreeNode *node = walk->elem;
				if (!node->followpos)
					node->followpos = newList();
				addList(node->followpos, first, REPEAT_BANNED);
			}

		freeList(first);
		freeList(last);
	}

	if (type != NULL_CHAR && type != LETTER && type != AUX)
		formFollowPosBy(node->left);

	if (type == CAT || type == OR)
		formFollowPosBy(node->right);

	if (type == LETTER)
		addToLetterList(node->val);
}

/*
根据抽象语法树和followpos集合生成对应的确定有穷自动机
*/
void generateDFA()
{
	Node *walk, *_walk;
	List *posSetCollection = newList();
	List *startPosSet = POSITION(tree, FIRST);
	DFAState *startState = newDFAState();

	dfa = newList();
	addElem(dfa, startState);
	addElem(posSetCollection, startPosSet);
	
	if (contains(startPosSet, tree->right))
		startState->accept = true;

	if (!letterList)
		goto L;

	for (walk = posSetCollection->head, _walk = dfa->head; walk; walk = walk->next, _walk = _walk->next) {	
		List *posSet = walk->elem;
		DFAState *state = _walk->elem;
		Node *walk1;
		for (walk1 = letterList->head; walk1; walk1 = walk1->next) {
			Node *walk;
			Char *ch = walk1->elem;
			List *newPosSet = newList();
			for (walk = posSet->head; walk; walk = walk->next) {
				TreeNode *node = walk->elem;
				if (node->val == ch->val)
					addList(newPosSet, node->followpos, REPEAT_BANNED);
			}
			if (newPosSet->head) {
				DFAState *gotoState;
				DFAMap *map = NULL, *tempMap = newDFAMap();
				List *gotoPosSet = equalElem(posSetCollection, newPosSet);
				if (gotoPosSet) {
					gotoState = nthElem(dfa, elemId(posSetCollection, gotoPosSet));
					tempMap->gotoState = gotoState;
					map = equalElem(state->mapList, tempMap);
					freeList(newPosSet);
				} else {
					gotoState = newDFAState();
					if (contains(newPosSet, tree->right))
						gotoState->accept = true;
					tempMap->gotoState = gotoState;
					addElem(posSetCollection, newPosSet);
					addElem(dfa, gotoState);
				}
				if (!map) {
					map = tempMap;
					if (!state->mapList)
						state->mapList = newList();
					addElem(state->mapList, map);
				} else
					free(tempMap);
				if (!map->symbolList)
					map->symbolList = newList();
				addElem(map->symbolList, ch);
			} else
				free(newPosSet);
		}
	}
L:
	freeList(posSetCollection);
	printf("DFA已生成，它有%d个状态\n", listSize(dfa));
}

/*
将生成的DFA化简为状态数最少的能区分所有模式的DFA
*/
void reduceDFA()
{
	Node *walk, *walk1;
	unsigned short i;
	unsigned short groupCount = 2;
	List **groups = emptyGroups(groupCount), *tempList;
	DFAState *startState = dfa->head->elem;

	for (walk = dfa->head; walk; walk = walk->next) {
		DFAState *state = walk->elem;
		addElem(groups[state->accept ? 0 : 1], state);
	}

	if (!groups[groupCount - 1]->head) {
		free(groups[groupCount - 1]);
		groups = realloc(groups, (--groupCount) * sizeof(List*));
	}

	containerOnly = true;
	freeList(dfa);

	for (i = 0; i < groupCount;) {
		unsigned short newCount;
		groups = divide(groups, groupCount, i);
		newCount = _msize(groups) / sizeof(List*);
		if (groupCount != newCount) {
			groupCount = newCount;
			i = 0;
		} else
			++i;
	}

	dfa = newList();

	for (i = 0; i < groupCount; ++i) {
		DFAState *newState = newDFAState();
		DFAState *state = groups[i]->head->elem;
		newState->accept = state->accept;
		addElem(dfa, newState);
	}

	for (i = 0; i < groupCount; ++i) {
		DFAState *newState = nthElem(dfa, i + 1);
		for (walk = groups[i]->head; walk; walk = walk->next) {
			DFAState *state = walk->elem;
			if (state->mapList)
				for (walk1 = state->mapList->head; walk1; walk1 = walk1->next) {
					DFAMap *map = walk1->elem, *newMap = NULL, *tempMap;
					unsigned short groupid = groupId(groups, groupCount, map->gotoState);
					DFAState *newGotoState = nthElem(dfa, groupid + 1);
					tempMap = newDFAMap(newGotoState);
					newMap = equalElem(newState->mapList, tempMap);
					if (!newMap) {
						newMap = tempMap;
						if (!newState->mapList)
							newState->mapList = newList();
						addElem(newState->mapList, newMap);
					} else
						free(tempMap);
					if (!newMap->symbolList)
						newMap->symbolList = newList();
					addList(newMap->symbolList, map->symbolList, REPEAT_BANNED);
				}
		}
	}

	startState = nthElem(dfa, groupId(groups, groupCount, startState) + 1);
	containerOnly = true;
	removeElem(dfa, startState);
	tempList = newList();
	addElem(tempList, startState);
	mergeList(tempList, dfa);
	dfa = tempList;

	freeGroups(groups, groupCount);
	printf("DFA最小化完成，它有%d个状态\n", listSize(dfa));
}

/*
对分划groups的第n+1个状态组进行分割
*/
List **divide(List **groups,
			  unsigned short groupCount,
			  unsigned short n)
{
	Node *walk, *walk1, *walk2;
	List **tempGroups = emptyGroups(groupCount + 1);
	
	for (walk = letterList->head; walk; walk = walk->next) {
		Char *ch = walk->elem;
		unsigned short divisionCount;
		for (walk1 = groups[n]->head; walk1; walk1 = walk1->next) {
			DFAState *state = walk1->elem;
			if (!state->mapList)
				addElem(tempGroups[groupCount], state);
			else {
				bool find = false;
				for (walk2 = state->mapList->head; walk2; walk2 = walk2->next) {
					DFAMap *map = walk2->elem;
					if (contains(map->symbolList, ch)) {
						unsigned short groupid = groupId(groups, groupCount, map->gotoState);
						addElem(tempGroups[groupid], state);
						find = true;
						break;
					}
				}
				if (!find)
					addElem(tempGroups[groupCount], state);
			}
		}
		divisionCount = noneEmptyGroupCount(tempGroups, groupCount + 1);
		if (divisionCount <= 1) {
			clearGroups(tempGroups, groupCount + 1);
			continue;
		} else {
			unsigned short i, j = nextGroupId(tempGroups, 0, groupCount + 1);
			groups = realloc(groups, (groupCount + divisionCount - 1) * sizeof(List*));
			containerOnly = true;
			for (i = n; i < groupCount + divisionCount - 1;) {
				if (i == n)
					freeList(groups[i]);
				groups[i] = tempGroups[j];
				tempGroups[j] = NULL;
				i = i == n ? groupCount : i + 1;
				j = nextGroupId(tempGroups, j + 1, groupCount + 1);
			}
			break;
		}
	}

	freeGroups(tempGroups, groupCount + 1);
	return groups;
}

/*
构造出拥有groupCount个空状态组的分划并返回
*/
List **emptyGroups(unsigned short groupCount)
{
	unsigned short i;
	List **groups = calloc(groupCount, sizeof(List*));

	for (i = 0; i < groupCount; ++i)
		groups[i] = newList();

	return groups;
}

/*
返回DFA状态state所在的组在分划groups中的序号
*/
unsigned short groupId(List **groups,
					   unsigned short groupCount,
					   DFAState *state)
{
	unsigned short i;

	for (i = 0; i < groupCount; ++i)
		if (contains(groups[i], state))
			return i;

	printf("分划groups中不存在要找的状态\n");
	exit(1);
}

/*
返回分划groups中非空状态组的个数
*/
unsigned short noneEmptyGroupCount(List **groups,
								   unsigned short groupCount)
{
	unsigned short i, count;

	for (i = count = 0; i < groupCount; ++i)
		if (groups[i]->head)
			++count;

	return count;
}

/*
清空分划groups（将其中的非空状态组置空）
*/
void clearGroups(List **groups, unsigned short groupCount)
{
	unsigned short i;
	containerOnly = true;

	for (i = 0; i < groupCount; ++i)
		if (groups[i]->head) {
			freeList(groups[i]);
			groups[i] = newList();
		}
}

/*
返回分划groups中从第thisId+1个状态组开始数遇到的
第一个非空状态组的下标，若到达groups数组的右边界
groupCount-1，则无论空还是非空，返回右边界下标
*/
unsigned short nextGroupId(List **groups,
						   unsigned short thisId,
						   unsigned short groupCount)
{
	unsigned short i;

	if (thisId >= groupCount - 1)
		return groupCount - 1;

	for (i = thisId; !groups[i]->head && i < groupCount - 1; ++i);

	return i;
}

/*
释放分划groups占据的空间
*/
void freeGroups(List **groups, unsigned short groupCount)
{
	unsigned short i;
	containerOnly = false;

	for (i = 0; i < groupCount; ++i)
		freeList(groups[i]);
	free(groups);

	containerOnly = true;
}

/*
运行DFA
*/
void runDFA()
{
	while (true) {
		char str[256];
		printf("请输入任意字符串：");
		gets(str);
		checkStr(str);
	}
}

/*
对字符串str运行自动机，看它能否被自动机接受
*/
void checkStr(char *str)
{
	char c;
	unsigned short i;
	DFAState *state = dfa->head->elem;
	
	if (!processPrimitiveStr(str, GENERAL))
		return;
	
	for (i = 0; c = str[i]; ++i) {
		if (!charType(c))
			continue;
		state = Goto(state, c);
		if (!state) {
			printf("DENIED\n");
			return;
		}
	}

	printf(state->accept ? "ACCEPTED\n" : "DENIED\n");
}

/*
返回自动机状态state在字符c的作用下到达的状态
*/
DFAState *Goto(DFAState *state, char c)
{
	Node *walk, *walk1;
	
	if (!state->mapList)
		return NULL;

	for (walk = state->mapList->head; walk; walk = walk->next) {
		DFAMap *map = walk->elem;
		if (map->symbolList)
			for (walk1 = map->symbolList->head; walk1; walk1 = walk1->next) {
				Char *ch = walk1->elem;
				if (ch->val == c)
					return map->gotoState;
			}
	}

	return NULL;
}

/*
释放堆区，并把所有数据恢复到开始时的状态
*/
void resume()
{
	containerOnly = false;

	freeStack(stack);
	freeList(dfa);
	freeList(numList);
	freeList(letterList);

	stack = NULL;
	dfa = NULL;
	numList = NULL;
	letterList = NULL;

	containerOnly = true;
	freeTree(tree);
	tree = NULL;

	listOrderConsidered = false;
	numCount = 0;
	fp = NULL;
}

/*
以c为值构造一个新的Char结构体
*/
Char *newChar(char val)
{
	Char *ch = calloc(1, sizeof(Char));
	ch->type = CHAR;
	ch->val = val;
	return ch;
}

/*
以val为值构造一个新的Short结构体
*/
Short *newShort(unsigned short val)
{
	Short *sh = calloc(1, sizeof(Short));
	sh->type = SHORT;
	sh->val = val;
	return sh;
}

/*
以state和attr为值构造一个新的StackRecord结构体
*/
Record *newRecord(unsigned short state, void *attr)
{
	Record *record = calloc(1, sizeof(Record));
	record->type = RECORD;
	record->state = state;
	record->attr = attr;
	return record;
}

/*
以mode、num1和num2为值构造一个新的Repeat结构体
*/
Repeat *newRepeat(RepeatMode mode, unsigned short num1, unsigned short num2)
{
	Repeat *repeat = calloc(1, sizeof(Repeat));
	repeat->type = _REPEAT;
	repeat->mode = mode;
	repeat->num1 = num1;
	repeat->num2 = num2;
	return repeat;
}

/*
以val为值构造一个新的TreeNode结构体
*/
TreeNode *newTreeNode(char val)
{
	TreeNode *node = calloc(1, sizeof(TreeNode));
	node->type = TREE_NODE;
	node->val = val;
	return node;
}

/*
以gotoState为值构造一个新的DFAMap结构体
*/
DFAMap *newDFAMap(DFAState *gotoState)
{
	DFAMap *map = calloc(1, sizeof(DFAMap));
	map->type = DFA_MAP;
	map->gotoState = gotoState;
	return map;
}

/*
构造一个新的DFAState结构体
*/
DFAState *newDFAState()
{
	DFAState *state = calloc(1, sizeof(DFAState));
	state->type = DFA_STATE;
	return state;
}

/*
构造一个新的List结构体
*/
List *newList()
{
	List *list = calloc(1, sizeof(List));
	list->type = LIST;
	return list;
}

/*
构造一个新的Stack结构体
*/
Stack *newStack()
{
	Stack *stack = calloc(1, sizeof(Stack));
	stack->type = STACK;
	return stack;
}

/*
规定了判断元素elem1和elem2是否相等的法则
*/
bool equals(void *elem1, void *elem2)
{
	Type type1, type2;

	if (elem1 == elem2)
		return true;

	if (!elem1 || !elem2)
		return false;

	type1 = *(Type*)elem1;
	type2 = *(Type*)elem2;

	if (type1 != type2)
		return false;

	switch (type1) {
		case CHAR:
		{
			Char *ch1 = elem1, *ch2 = elem2;
			return ch1->val == ch2->val;
		}
		case SHORT:
		{
			Short *sh1 = elem1, *sh2 = elem2;
			return sh1->val == sh2->val;
		}
		case DFA_MAP:
		{
			DFAMap *map1 = elem1, *map2 = elem2;
			return map1->gotoState == map2->gotoState;
		}
		case LIST:
		{
			unsigned short size1 = listSize(elem1);
			unsigned short size2 = listSize(elem2);
			
			List *list1 = elem1, *list2 = elem2;
			Node *walk1, *walk2;

			if (size1 != size2)
				return false;
		
			if (listOrderConsidered) {
				for (walk1 = list1->head, walk2 = list2->head;
					walk1 && walk2; walk1 = walk1->next, walk2 = walk2->next)
					if (!equals(walk1->elem, walk2->elem))
						break;
				return walk1 == walk2;
			} else {
				/*此处判断2个List是否相等（不考虑元素的排列顺序，
				  只要含有相同的元素即相等，此算法对包含重复元素的list无效）*/
				for (walk1 = list1->head; walk1; walk1 = walk1->next) {
					bool flag = false;
					for (walk2 = list2->head; walk2; walk2 = walk2->next)
						if (equals(walk1->elem, walk2->elem)) {
							flag = true;
							break;
						}
					if (!flag)
						return false;
				}
				return true;
			}
		}
		default:
			return elem1 == elem2;
	}
}

/*
返回list中第1个与elem相等的元素
*/
void *equalElem(List *list, void *elem)
{
	Node *walk;
	
	if (!list)
		return NULL;

	for (walk = list->head; walk; walk = walk->next)
		if (equals(walk->elem, elem))
			return walk->elem;
	
	return NULL;
}

/*
返回元素elem在list中的序号
*/
unsigned short elemId(List *list, void *elem)
{
	Node *walk;
	unsigned short i;

	if (!list || !elem)
		return 0;

	for (walk = list->head, i = 1; walk; walk = walk->next, ++i)
		if (equals(elem, walk->elem))
			return i;

	return 0;
}

/*
返回集合list中的第n个元素
*/
void *nthElem(List *list, unsigned short n)
{
	unsigned short i;
	Node *walk;

	if (!list || !n)
		return NULL;

	for (walk = list->head, i = 1; walk; walk = walk->next, ++i)
		if (i == n)
			return walk->elem;

	return NULL;
}

/*
返回元素elem的克隆体
*/
void *clone(void *elem)
{
	switch (*(Type*)elem) {
		case CHAR:
			return newChar(((Char*)elem)->val);
		case SHORT:
			return newShort(((Short*)elem)->val);
		default:
			return NULL;
	}
}

/*
将集合list2中的元素并入集合list1，list2保持不变
*/
void addList(List *list1, List *list2, MergeType mergeType)
{
	Node *walk;

	if (!list1 || !list2)
		return;
	
	for (walk = list2->head; walk; walk = walk->next) {
		void *newElem = walk->elem;
		int type = *(Type*)walk->elem;
		if (type == CHAR || type == SHORT)
			newElem = clone(walk->elem);
		if (mergeType == REPEAT_ALLOWED || !contains(list1, walk->elem))
			addElem(list1, newElem);
	}
}

/*
将集合list2中的元素并入集合list1，删除空集list2
*/
void mergeList(List *list1, List *list2)
{
	if (!list1)
		list1 = list2;
	else if (list2) {
		if (list2->head) {
			list1->tail->next = list2->head;
			list1->tail = list2->tail;
		}
		free(list2);
	}
}

/*
将元素elem添加进集合list
*/
void addElem(List *list, void *elem)
{
	Node *newNode = NULL;
	
	if (!list || !elem)
		return;

	newNode = calloc(1, sizeof(Node));
	newNode->elem = elem;

	if (!list->head)
		list->head = list->tail = newNode;
	else {
		list->tail->next = newNode;
		list->tail = newNode;
	}
}

/*
将元素elem从集合list中删除
*/
void removeElem(List *list, void *elem)
{
	Node **walk = NULL, *prev;

	if (!list)
		return;

	for (prev = NULL, walk = &list->head; *walk;)
		if ((*walk)->elem == elem) {
			Node *node = *walk;
			*walk = node->next;
			if (!containerOnly)
				freeElem(node->elem);
			else if (*(Type*)node->elem == LIST)
				freeList(node->elem);
			else if (*(Type*)node->elem == STACK)
				freeStack(node->elem);
			free(node);
			if (node == list->tail)
				list->tail = prev;
			break;
		} else {
			prev = *walk;
			walk = &(*walk)->next;
		}
}

/*
释放元素elem及其相关的内存空间
*/
void freeElem(void *elem)
{
	if (!elem)
		return;

	switch (*(Type*)elem) {
		case CHAR:
		case SHORT:
		case _REPEAT:
			free(elem);
			break;
		case RECORD:
		{
			Record *record = elem;
			freeElem(record->attr);
			break;
		}
		case TREE_NODE:
			containerOnly = true;
			freeTree(elem);
			break;
		case DFA_MAP:
		{
			DFAMap *map = elem;
			freeList(map->symbolList);
			free(map);
			break;
		}
		case DFA_STATE:
		{
			DFAState *state = elem;
			freeList(state->mapList);
			free(state);
			break;
		}
		case LIST:
			freeList(elem);
			break;
		case STACK:
			freeStack(elem);
	}
}

/*
释放集合list，若containerOnly
为false，则其中的元素也要释放
*/
void freeList(List *list)
{
	if (!list)
		return;

	while (list->head) {
		Node *node = list->head;
		list->head = node->next;
		if (!containerOnly)
			freeElem(node->elem);
		else if (*(Type*)node->elem == LIST)
			freeList(node->elem);
		else if (*(Type*)node->elem == STACK)
			freeStack(node->elem);
		free(node);
	}

	free(list);
}

/*
释放栈stack，若containerOnly
为false，则其中的元素也要释放
*/
void freeStack(Stack *stack)
{
	void *elem;

	if (!stack)
		return;

	while (elem = pop(stack)) {
		if (!containerOnly)
			freeElem(elem);
		else if (*(Type*)elem == LIST)
			freeList(elem);
		else if (*(Type*)elem == STACK)
			freeStack(elem);
	}

	free(stack);
}

/*
返回集合list中的元素的数量
*/
unsigned short listSize(List *list)
{
	unsigned short size = 0;
	Node *node;

	if (!list)
		return 0;

	node = list->head;

	while (node) {
		++size;
		node = node->next;
	}

	return size;
}

/*
将元素elem压入栈stack
*/
void push(Stack *stack, void *elem)
{
	Node *node = calloc(1, sizeof(Node));
	node->elem = elem;
	node->next = stack->top;
	stack->top = node;
}

/*
弹出栈stack的栈顶元素
*/
void *pop(Stack *stack)
{
	Node *node = stack->top;
	void *elem;

	if (!node)
		return NULL;

	elem = node->elem;
	stack->top = node->next;
	free(node);
	return elem;
}

/*
将离栈顶有n个记录远的记录返回
*/
void *stackElem(Stack *stack, unsigned short n)
{
	void *elem = NULL;
	Node *node = stack->top;
	unsigned short i = 0;

	while (node && i < n) {
		node = node->next;
		++i;
	}

	if (node)
		elem = node->elem;

	return elem;
}

/*
释放以node为根的树的空间
*/
void freeTree(TreeNode *node)
{
	if (node) {
		freeTree(node->left);
		freeTree(node->right);
		freeList(node->followpos);
		free(node);
	}
}

/*
打印元素elem
*/
void printElem(void *elem)
{
	if (!elem)
		return;

	switch (*(Type*)elem) {
		case CHAR:
			printf("%c\n", ((Char*)elem)->val);
			break;
		case SHORT:
			printf("%d\n", ((Short*)elem)->val);
			break;
		case LIST:
			printList(elem);
	}
}

/*
打印集合list
*/
void printList(List *list)
{
	Node *walk = NULL;

	if (!list)
		return;

	for (walk = list->head; walk; walk = walk->next)
		printElem(walk->elem);
}








