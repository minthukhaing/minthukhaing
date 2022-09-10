- 👋 Hi, I’m @minthukhaing
- 👀 I’m interested in ...
- 🌱 I’m currently learning ...
- 💞️ I’m looking to collaborate on ...
- 📫 How to reach me ...

<!---
minthukhaing/minthukhaing is a ✨ special ✨ repository because its `README.md` (this file) appears on your GitHub profile.
You can click the Preview link to take a look at your changes.
--->
Само сложение заключается в коротком цикле:
For i:=1 to N do    // Цикл по количеству разрядов
  Begin
   Sum[i]:=(A[i]+B[i]+carry) mod Radix[i];        // Сумма разрядов
  carry:=(A[i]+B[i]+carry) div Radix[i];         // перенос в следующий разряд
  End;

Обращаю внимание, что в полях ввода все числа записываются (через пробел) младшими разрядами вперёд!

Это кусочек программы от Якунина

08606E811EC0

  
