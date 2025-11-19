## Javadoc for StringUtil.insert(...)

Este é o Javadoc completo, formatado para a entrega:

```java
/**
* Inserts a specified string into another string at a designated offset.
*
* If the original string (s) is null, the method returns the literal string "null".
* If the original string (s) is empty, the method returns the insert string.
* If the offset is greater than the length of the string, the string to insert
* is appended to the end of the original string.
*
* @param s The string to insert into. If null, the method returns "null".
* @param insert The string to be inserted.
* @param offset The index at which the insertion should occur. This index is zero-based.
* @return The new string with the insertion, or the literal string "null" if s is null.
*/
public static String insert(String s, String insert, int offset) {
...
}
