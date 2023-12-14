# base64编解码

#### JDK的sun.misc套件

```java
final BASE64Encoder encoder = new BASE64Encoder();
final BASE64Decoder decoder = new BASE64Decoder();
final String text = "字符串文字";
final byte[] textByte = text.getBytes("UTF-8");
//编码
final String encodedText = encoder.encode(textByte);
System.out.println(encodedText);
//解码
System.out.println(new String(decoder.decodeBuffer(encodedText), "UTF-8"));
```

#### Apache Commons Codec

```java
final Base64 base64 = new Base64();
final String text = "字串文字";
final byte[] textByte = text.getBytes("UTF-8");
//编码
final String encodedText = base64.encodeToString(textByte);
System.out.println(encodedText);
//解码
System.out.println(new String(base64.decode(encodedText), "UTF-8"));
```

#### Java8的java.util套件

```java
final Base64.Decoder decoder = Base64.getDecoder();
final Base64.Encoder encoder = Base64.getEncoder();
final String text = "字串文字";
final byte[] textByte = text.getBytes("UTF-8");
//编码
final String encodedText = encoder.encodeToString(textByte);
System.out.println(encodedText);
//解码
System.out.println(new String(decoder.decode(encodedText), "UTF-8"));
```