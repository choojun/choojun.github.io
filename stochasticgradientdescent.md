
```
public class SGD
{
	// https://metamug.com/article/stochastic-gradient-descent-tutorial-code-by-andrew-ng.html
	// http://cs229.stanford.edu/notes/cs229-notes1.pdf
	
	public static void main(String[] args)
	{
		java.util.Random r = new java.util.Random();
		double[][] x = new double[5][2];
		double[] y = new double[5];
		for (int i = 0; i < x.length; i++)
		{
			y[i] = r.nextDouble();
			for (int j = 0; j < x[i].length; j++)
				x[i][j] = r.nextDouble();
		}

		SGD myObj = new SGD();
		myObj.stochastic(x, y);

		for (int i = 0; i < myObj.c.length; i++)
		{
			System.out.println(i+" coefficients: "+myObj.c[i]);
		}
		String data = "y: ";
		for (int i = 0; i < y.length; i++)
			data += y[i] + " ";
		data += "\n";
		data += "x: ";
		for (int i = 0; i < x.length; i++)
		{
			for (int j = 0; j < x[i].length; j++)
				data += x[i][j] + " ";
			
			data += "\n";
		}
		System.out.println(data);
	}

	final double a = 0.000001; // value of alpha
	double[] c;// coefficients

	strictfp void stochastic(double[][] x, double[] y)
	{
		c = new double[x[0].length]; // constants init as 0.0
		double[] h = new double[y.length]; // heuristics same as y
		while (examine(y, h) > 0.1)
		{
			for (int i = 0; i < y.length; i++)
			{
				// 1. calculate heuristic
				h[i] = c[0];
				for (int j = 0; j < c.length; j++)
				{
					h[i] += c[j] * x[i][j];
				}

				// 2. Calculate Error
				double delta = (y[i] - h[i]);

				// 3. update constants
				c[0] += a * delta;
				for (int j = 0; j < c.length; j++)
				{
					// update cj for xj
					c[j] += a * delta * x[i][j];
				}
			}
		}
	}

	strictfp double examine(double[] y, double[] h)
	{
		    double sum = 0.0 ; 
		    // minimizing the sum of squares
		    for(int i=0; i<y.length; i++)
		    { 
		        sum += (y[i] - h[i])*2;
		    }
		    sum = sum/2;

		    //negligible difference between errors in two consecutive iterations
		    //assign the error to be minimized to 0
		    double prevSum = 0.2;
		    if(Math.abs(prevSum - sum)< 10*-10)
		    {
				sum = 0;
		    }
		    prevSum = sum; //for taking difference in iteration    

		    //converge if error
		    System.out.println(sum);
		    return sum; 
		
	}
}
```

Sample output
```
0.13447128305112405
0.13447014640427063
0.13446900977235293
0.13446787315537206
0.13446673655332647
...
0.10002465907268743
0.10002397826120041
0.10002329745881139
0.10002261666551926
0.10002193588132502
0.10002125510622689
0.10002057434022532
0.10001989358332092
0.10001921283551402
0.10001853209680339
0.10001785136718994
0.10001717064667281
0.10001648993525147
0.10001580923292641
0.10001512853969746
0.10001444785556446
0.1000137671805279
0.10001308651458679
0.10001240585774102
0.10001172520999135
0.1000110445713368
0.10001036394177804
0.10000968332131416
0.100009002709945
0.10000832210767069
0.10000764151449193
0.10000696093040784
0.10000628035541781
0.10000559978952228
0.10000491923272059
0.10000423868501324
0.10000355814639994
0.10000287761688054
0.1000021970964558
0.10000151658512468
0.10000083608288718
0.1000001555897434
0.09999947510569313
0 coefficients: 0.4005561306857344
1 coefficients: 0.023834337424821276
y: 0.5768475767586754 0.3119882830391444 0.3393336873880386 0.962443507463582 0.9164396181463513 
x: 0.4220029892910059 0.7801544655778861 
0.4753830847769538 0.8725276253376977 
0.07062472628546212 0.9195098033273317 
0.37895721429206586 0.109190997705092 
0.9836687005783521 0.2859382602981174 
```
