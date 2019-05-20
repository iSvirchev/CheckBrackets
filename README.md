package com.company;

import java.util.ArrayDeque;
import java.util.regex.Matcher;
import java.util.regex.Pattern;

public class Main {

    public static void main(String[] args) {
        String textTrue = "This is {some (text), nq[ma ](kakvo na 890 ti ,l;';') [da ;l'l'] za vas}";
        String textFalse = "Tdfhhis is {some (texghft),123 nqma ([kakvo]gh na asdsad ti dasd) r6r kaja ( za bvcws) }";
        String textFalse2 = "This asdgfis {some (texfght), nfgnqma ([kkakvo] na asd678sadsdf ti erfgdfgdfgd df) da fgh ( za [] vas) }";
        String textFalse3 = "This isds {some (text), .,/789 ([kakvo]mbv na asdasd ti asdasd) da asdasd ( za {} asdasd) }";
        String textFalse4 = "This is {some (textsas), nqma ([ka{}kvo] nvbnvba dfvdfg ti dfgdfg) da kaja ( za vas) }";
        String noBrackets = "this is some jklkjljklj jkljkljkl jkl43234assd"; 

        String[] test = new String[]{
                textTrue, textFalse, textFalse2, textFalse3, textFalse4, noBrackets
        };

        for (int i = 0; i < test.length; i++) {
            System.out.println(CheckBrackets(test[i]));
        }
    }

    private static boolean CheckBrackets(String textTrue) {
        String bracketsRegex = "(?<openingRound>[\\(])?(?<closingRound>[\\)])?(?<openingSquare>[\\[])?(?<closingSquare>[\\]])?(?<openingCurly>[\\{])?(?<closingCurly>[\\}])?";

        Pattern pattern = Pattern.compile(bracketsRegex);
        Matcher matcher = pattern.matcher(textTrue);

        StringBuilder sb = new StringBuilder();

        while (matcher.find()) {
            if (!(matcher.group("openingRound") == null)) {
                sb.append(matcher.group("openingRound"));
            } else if (!(matcher.group("closingRound") == null)) {
                sb.append(matcher.group("closingRound"));
            } else if (!(matcher.group("openingSquare") == null)) {
                sb.append(matcher.group("openingSquare"));
            } else if (!(matcher.group("closingSquare") == null)) {
                sb.append(matcher.group("closingSquare"));
            } else if (!(matcher.group("openingCurly") == null)) {
                sb.append(matcher.group("openingCurly"));
            } else if (!(matcher.group("closingCurly") == null)) {
                sb.append(matcher.group("closingCurly"));
            }
        }
        boolean isValid = true;
        if (sb.length() % 2 != 0) {
            isValid = false;
        }
        String brackets = sb.toString();
        
        ArrayDeque<Character> stack = new ArrayDeque<>();

        for (int i = 0; i < brackets.length(); i++) {
            char current = brackets.charAt(i);

            if (current == '[' || current == '{' || current == '(') {
                stack.push(current);
            } else {
                if (stack.isEmpty()) {
                    isValid = false;
                    break;
                }
                char topChar = stack.pop();
                if (current == ']' && topChar != '[') {
                    isValid = false;
                    break;
                } else if (current == '}' && topChar != '{') {
                    isValid = false;
                    break;
                } else if (current == ')' && topChar != '(') {
                    isValid = false;
                    break;
                }
            }
        }
        return isValid;
    }
}
