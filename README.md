# 05StackAndQueue

20. 有效的括号

给定一个只包括 '('，')'，'{'，'}'，'['，']' 的字符串，判断字符串是否有效。

有效字符串需满足：

左括号必须用相同类型的右括号闭合。
左括号必须以正确的顺序闭合。
注意空字符串可被认为是有效字符串。
示例 1:

输入: "()"
输出: true

解题思路：
匹配连续相同类型的题目非常适合使用栈来完成，因为栈顶元素就相当于字符串的当前数值，可以用它与下一个进行比较。这题我们入栈可以存（，[,{也可以存配对的，如果选择配的其实在栈顶跟遍历的下一个配对的时候会方便很多，直接看是否相等就行了。
然后分析，只有三种情况不匹配。1.开始（一开始或者前面刚刚好配对完）就是闭符号。2.下一个来配对的跟栈顶不一样。3.字符串遍历结束了，栈里还有没消除的。

代码实现：
class Solution {
    public boolean isValid(String s) {
        Deque<Character> deque = new LinkedList<>();
        char ch;
        for (int i = 0; i < s.length(); i++) {
            ch = s.charAt(i);
            //碰到左括号，就把相应的右括号入栈
            if (ch == '(') {
                deque.push(')');
            }else if (ch == '{') {
                deque.push('}');
            }else if (ch == '[') {
                deque.push(']');
            } else if (deque.isEmpty() || deque.peek() != ch) {
                return false;
            }else {//如果是右括号判断是否和栈顶元素匹配
                deque.pop();
            }
        }
        //最后判断栈中元素是否匹配
        return deque.isEmpty();
    }
}

1047. 删除字符串中的所有相邻重复项

给出由小写字母组成的字符串 S，重复项删除操作会选择两个相邻且相同的字母，并删除它们。

在 S 上反复执行重复项删除操作，直到无法继续删除。

在完成所有重复项删除操作后返回最终的字符串。答案保证唯一。

示例：

输入："abbaca"
输出："ca"
解释：例如，在 "abbaca" 中，我们可以删除 "bb" 由于两字母相邻且相同，这是此时唯一可以执行删除操作的重复项。之后我们得到字符串 "aaca"，其中又只有 "aa" 可以执行重复项删除操作，所以最后的字符串为 "ca"。

解题思路：匹配连续相同类可以用栈实现，栈也可以用String来实现，每次对比新string的最后一位与loop string的当前字符对比，相同则消除，不同则加入。
本体还可以用一个指针top来记录字符串的最顶端，这样就不需要每次调用size来去栈顶元素，可以节省一些loop时间。最早把top标记为0，第一个字符存到新字符串中，然后每次加字符就++；消除字符就--；

代码实现：
class Solution {
    public String removeDuplicates(String s) {
        StringBuilder sb = new StringBuilder();
        for(int i= 0; i < s.length(); i++){
            char ch = s.charAt(i);
            if(sb.length() == 0 || ch != sb.charAt(sb.length()-1)){
                sb.append(ch);
            }else{
                sb.deleteCharAt(sb.length()-1);
            }
        }
        return sb.toString();
    }
}

class Solution {
    public String removeDuplicates(String s) {
        // 将 res 当做栈
        // 也可以用 StringBuilder 来修改字符串，速度更快
        // StringBuilder res = new StringBuilder();
        StringBuffer res = new StringBuffer();
        // top为 res 的长度
        int top = 0;
        for (int i = 1; i < s.length(); i++) {
            char c = s.charAt(i);
            // 当 top >= 0,即栈中有字符时，当前字符如果和栈中字符相等，弹出栈顶字符，同时 top--
            if (top >= 0 && res.charAt(top) == c) {
                res.deleteCharAt(top);
                top--;
            // 否则，将该字符 入栈，同时top++
            } else {
                res.append(c);
                top++;
            }
        }
        return res.toString();
    }
}
