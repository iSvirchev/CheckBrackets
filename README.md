package com.company;

import java.util.Optional;
import java.util.regex.Matcher;
import java.util.regex.Pattern;

public class Main {

    static boolean goodBrackets = false;
    static boolean round = false;
    static boolean square = false;
    static boolean curly = false;

    public static void main(String[] args) {
        // The task is to check the hierarchy of the brackets and to check if every open bracket is closed
        //  - only () can be inside ()
        //  - only [] and () can be inside []
        //  - all can be inside {}

        String textTrue = "This is {some (text), nq[ma ](kakvo na maika ti putkata) [da kaja] za vas}";
        String textFalse = "This is {some (text), nqma ([kakvo] na maika ti putkata) da kaja ( za vas) }";
        String textFalse2 = "This is {some (text), nqma ([kakvo] na maika ti putkata) da kaja ( za [] vas) }";
        String textFalse3 = "This is {some (text), nqma ([kakvo] na maika ti putkata) da kaja ( za {} vas) }";
        String textFalse4 = "This is {some (text), nqma ([ka{}kvo] na maika ti putkata) da kaja ( za vas) }";
        String noBrackets = "this is some motherfucking text bitch";

        String[] test = new String[]{
                textTrue, textFalse, textFalse2, textFalse3, textFalse4, noBrackets
        };

       /* for (int i = 0; i < test.length; i++) {
            goodBrackets = false;
            CheckBreckets(test[i]);
            System.out.println(goodBrackets);
        }*/

    }

    private static void CheckBreckets(String textTrue) {
        String bracketsRegex = "(?<openingRound>[\\(])?(?<closingRound>[\\)])?(?<openingSquare>[\\[])?(?<closingSquare>[\\]])?(?<openingCurly>[\\{])?(?<closingCurly>[\\}])?";

        Pattern pattern = Pattern.compile(bracketsRegex);
        Matcher matcher = pattern.matcher(textTrue);

        StringBuilder sb = new StringBuilder();

        int openRound = 0;
        int closeRound = 0;
        int openSquare = 0;
        int closeSquare = 0;
        int openCurly = 0;
        int closeCurly = 0;

        while (matcher.find()) {
            if (!(matcher.group("openingRound") == null)) {
                sb.append(matcher.group("openingRound"));
                openRound++;
            } else if (!(matcher.group("closingRound") == null)) {
                sb.append(matcher.group("closingRound"));
                closeRound++;
            } else if (!(matcher.group("openingSquare") == null)) {
                sb.append(matcher.group("openingSquare"));
                openSquare++;
            } else if (!(matcher.group("closingSquare") == null)) {
                sb.append(matcher.group("closingSquare"));
                closeSquare++;
            } else if (!(matcher.group("openingCurly") == null)) {
                sb.append(matcher.group("openingCurly"));
                openCurly++;
            } else if (!(matcher.group("closingCurly") == null)) {
                sb.append(matcher.group("closingCurly"));
                closeCurly++;
            }
        }

        if (openRound == closeRound && openSquare == closeSquare && openCurly == closeCurly) {
            if (openCurly == 0) curly = true;
            if (openSquare == 0) square = true;
            if (openRound == 0) round = true;

            if (curly && square && round) {
                goodBrackets = true;
                return;
            }

            for (int i = 0; i < sb.length() - 1; i++) {
                CheckInside(sb, i);
            }
        }

    }

    private static void CheckInside(StringBuilder sb, int i) {
        switch (sb.charAt(i)) {
            case '(':
                if (sb.charAt(i + 1) == '(') CheckInside(sb, i + 1);
                else if (sb.charAt(i + 1) == ')') round = true;
                else round = false;
                break;
            case '[':
                if (sb.charAt(i + 1) == '[' || sb.charAt(i + 1) == '(') CheckInside(sb, i + 1);
                else if (sb.charAt(i + 1) == ']') square = true;
                else square = false;
                break;
            case '{':
                if (sb.charAt(i + 1) == '{' || sb.charAt(i + 1) == '[' || sb.charAt(i + 1) == '(')
                    CheckInside(sb, i + 1);
                else if (sb.charAt(i + 1) == '}') curly = true;
                else curly = false;
                break;
        }
    }
}
