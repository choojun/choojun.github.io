### Download Eclipse offline version
http://www.eclipse.org/downloads/eclipse-packages/

### Java Fuzzy Markup Language
http://www.uco.es/JFML/home

### Top 16 Java Utility Classes 
[Read this](http://www.programcreek.com/2015/12/top-10-java-utility-classes/).

### [Read this](http://www.know.bi/blog/article) for the Weka plugins for PDI (Pentaho Data Integration): Scoring and Forecasting

### LD_LIBRARY_PATH and Eclipse
```
use Eclipse to set environment variable LD_LIBRARY_PATH.
You do it from the "Run..." or "Debug..." dialog, in the "Environment" tab.

Example:
Name: LD_LIBRARY_PATH
Value: /home/choojun/Downloads/dist_linux_x86_64/
```

### Threads and available processors
```
int numberOfThreads_ = Runtime.getRuntime().availableProcessors() ;
```

### GPU Programming and Java
Be aware of the fact that CUDA/OpenCL will not automagically make computations faster. GPU programming is an art, and it can be very, very challenging to get it right. It is worth in noting that GPUs are well-suited only for certain kinds of computations.

**Note 1**: You may compute anything on the GPU. However, whether you will achieve a good speedup or not is an issue. It is due to a problem of 'task parallel' or 'data parallel'. Task parallel refers to problems where several threads are working on their own tasks, more or less independently. Data parallel refers to problems where many threads are all doing the same - but on different parts of the data. Note that the latter is the kind of problem that GPUs are good at: They have many cores, and all the cores do the same, but operate on different parts of the input data. 

**Note 2**: "Simple math but with huge amount of data": it sound like a perfectly data-parallel problem and thus like it was well-suited for a GPU. It is no doubts and agreed that GPUs are ridiculously fast in terms of theoretical computational power (FLOPS, Floating Point Operations Per Second). However, they are often throttled down by the memory bandwidth which are categorised into memory bound and compute bound.

Memory bound refers to problems where the number of instructions that are done for each data element is low. For example, consider a parallel vector addition: You'll have to read two data elements, then perform a single addition, and then write the sum into the result vector. You will not see a speedup when doing this on the GPU, because the single addition does not compensate for the efforts of reading/writing the memory.

Compute bound refers to problems where the number of instructions is high compared to the number of memory reads/writes. For example, consider a matrix multiplication: The number of instructions will be O(n^3) when n is the size of the matrix. In this case, one can expect that the GPU will outperform a CPU at a certain matrix size. Another example could be when many complex trigonometric computations (sine/cosine etc) are performed on "few" data elements. 

**Note 3**: Suppose that reading/writing one data element from the "main" GPU memory has a latency of about 500 instructions....
GPUs is data locality: always kept as close as possible to the GPU cores. GPUs have certain memory areas, i.e. referred to as "local memory" or "shared memory", that usually is only a few KB in size, but particularly efficient for data that is about to be involved in a computation.

Threads in Java, with all the concurrency infrastructure, give the impression that we just have to split work and distribute it among several processors. On the other hand, GPU programming is an art. We encounter challenges on a much lower level with GPU programming Occupancy, register pressure, shared memory pressure, memory coalescing ... just to name a few CUDA examples over here :D 

In summary, when you have a data-parallel, compute-bound problem to solve, the GPU is the way to go. 


### Output Boxplot in an SVG file
As a Maven project, it requires the following libraries.
```
<dependency>
	<groupId>org.apache.xmlgraphics</groupId>
	<artifactId>batik-svggen</artifactId>
	<version>1.9.1</version>
</dependency>
<dependency>
	<groupId>org.apache.xmlgraphics</groupId>
	<artifactId>batik-transcoder</artifactId>
	<version>1.9.1</version>
</dependency>
<dependency>
	<groupId>batik</groupId>
	<artifactId>batik-dom</artifactId>
	<version>1.6-1</version>
</dependency>
<dependency>
	<groupId>org.apache.xmlgraphics</groupId>
	<artifactId>batik-codec</artifactId>
	<version>1.9</version>
</dependency>
```

Create the following class as the application.
```
import java.awt.Color;
import java.awt.Graphics2D;
import java.io.File;
import java.io.FileWriter;
import java.io.Writer;

import org.apache.batik.dom.GenericDOMImplementation;
import org.apache.batik.svggen.DOMGroupManager;
import org.apache.batik.svggen.ExtensionHandler;
import org.apache.batik.svggen.ImageHandler;
import org.apache.batik.svggen.SVGGeneratorContext;
import org.apache.batik.svggen.SVGGraphics2D;
import org.apache.batik.util.SVGConstants;
import org.w3c.dom.DOMImplementation;
import org.w3c.dom.Document;
import org.w3c.dom.Element;

public class BoxPlot
{

	public static int overallScale = 10;
	public static int bpHeight = overallScale * 5;

	public static void main(String[] args)
	{
		BoxPlot fictionPrice = new BoxPlot("Fiction");
		fictionPrice.q1 = 5.24;
		fictionPrice.q2 = 5.59;
		fictionPrice.q3 = 6.29;
		fictionPrice.low = 3.99;
		fictionPrice.high = 7.00;
		fictionPrice.outliers = new double[]
		{ 13.60, 9.09, 12.91, 8.39, 10.49, 8.55, 1.00 };

		BoxPlot nonfictionPrice = new BoxPlot("Non fiction");
		nonfictionPrice.q1 = 5.565;
		nonfictionPrice.q2 = 6.99;
		nonfictionPrice.q3 = 10.49;
		nonfictionPrice.low = 1.00;
		nonfictionPrice.high = 17.68;
		nonfictionPrice.outliers = new double[]
		{ 26.00 };

		BoxPlot childrensPrice = new BoxPlot("Children's ");
		childrensPrice.q1 = 4.89;
		childrensPrice.q2 = 5.24;
		childrensPrice.q3 = 5.24;
		childrensPrice.low = 4.49;
		childrensPrice.high = 5.59;
		childrensPrice.outliers = new double[]
		{ 6.49, 5.84, 8.24, 7.69, 3.99, 20.40, 3.38, 7.17, 7.00 };

		generate("price", 0, 30, 5, 10, "£", "", fictionPrice, nonfictionPrice, childrensPrice);

	}
	
	

	public static void generate(String filename, int chartMin, int chartMax, int interval, double scale,
			String unitBefore, String unitAfter, BoxPlot... boxPlots)
	{
		DOMImplementation domImpl = GenericDOMImplementation.getDOMImplementation();

		String svgNS = "http://www.w3.org/2000/svg";
		Document document = domImpl.createDocument(svgNS, "svg", null);

		SVGGraphics svgGenerator = new SVGGraphics(document);

		int height = (int) (((boxPlots.length * 1.5) + 0.5) * bpHeight);
		int xoffset = bpHeight * 2;
		// grid
		svgGenerator.setPaint(Color.LIGHT_GRAY);
		for (int i = 1; i <= chartMax; i += (interval / 5))
		{
			svgGenerator.drawLine((int) (xoffset + (i * scale)), 1, (int) (xoffset + (i * scale)), height);
		}

		svgGenerator.setPaint(Color.BLACK);
		// x axis
		svgGenerator.drawLine(xoffset, 1, xoffset, height);
		// y axis
		svgGenerator.drawLine(xoffset, height, (int) (xoffset + (chartMax - chartMin) * scale), height);
		// x axis labels
		for (int i = 0; i <= chartMax; i += interval)
		{
			anchoredText(svgGenerator, unitBefore + i + unitAfter, (int) (xoffset + (i * scale)),
					height + (int) (bpHeight * 0.25), "middle");
		}

		for (int i = 0; i < boxPlots.length; i++)
		{
			// label
			anchoredText(svgGenerator, boxPlots[i].name, xoffset - overallScale, (int) (((i * 1.5) + 1) * bpHeight),
					"end");
			// draw
			boxPlots[i].paint(svgGenerator, scale, xoffset, (int) (((i * 1.5) + 0.5) * bpHeight));
		}

		try
		{
			// write svg
			Writer svgOut = new FileWriter(new File(filename + ".svg"));
			svgGenerator.stream(svgOut, true);
			svgOut.close();

			/*
			// write png
			Rectangle areaOfInterest = new Rectangle((int) (xoffset + chartMax * scale + bpHeight),
					(int) (height + 5 * scale + bpHeight));

			PNGTranscoder p = new PNGTranscoder();
			p.addTranscodingHint(ImageTranscoder.KEY_PIXEL_UNIT_TO_MILLIMETER, 1f);
			p.addTranscodingHint(ImageTranscoder.KEY_WIDTH, areaOfInterest.width * 10f);
			p.addTranscodingHint(ImageTranscoder.KEY_HEIGHT, areaOfInterest.height * 10f);
			p.addTranscodingHint(ImageTranscoder.KEY_AOI, areaOfInterest);
			Reader svgIn = new FileReader(new File(filename + ".svg"));
			TranscoderInput input = new TranscoderInput(svgIn);
			FileOutputStream pngOut = new FileOutputStream(new File(filename + ".png"));
			TranscoderOutput output = new TranscoderOutput(pngOut);
			p.transcode(input, output);
			svgIn.close();
			pngOut.close();
			*/
		}
		catch (Exception e)
		{
			e.printStackTrace();
		}
	}

	public static void anchoredText(SVGGraphics svgGenerator, String string, int x, int y, String textAnchor)
	{
		Element text = svgGenerator.getDOMFactory().createElementNS(SVGConstants.SVG_NAMESPACE_URI,
				SVGConstants.SVG_TEXT_TAG);
		text.setAttributeNS(null, SVGConstants.SVG_X_ATTRIBUTE, svgGenerator.generatorCtx().doubleString(x));
		text.setAttributeNS(null, SVGConstants.SVG_Y_ATTRIBUTE, svgGenerator.generatorCtx().doubleString(y));
		// center text
		text.setAttributeNS(null, "text-anchor", textAnchor);

		text.setAttributeNS(SVGConstants.XML_NAMESPACE_URI, SVGConstants.XML_SPACE_QNAME,
				SVGConstants.XML_PRESERVE_VALUE);
		text.appendChild(svgGenerator.getDOMFactory().createTextNode(string));
		svgGenerator.domGroupManager().addElement(text, DOMGroupManager.FILL);
	}

	public String name;
	public double[] outliers;
	public double q1;
	public double q2;
	public double q3;
	public double low;
	public double high;

	public BoxPlot(String s)
	{
		this.name = s;
	}

	public void paint(Graphics2D g2d, double scale, int xoffset, int yoffset)
	{
		// q1 line
		g2d.drawLine(xoffset + (int) (q1 * scale), yoffset, xoffset + (int) (q1 * scale), yoffset + bpHeight);
		// q2 line
		g2d.drawLine(xoffset + (int) (q2 * scale), yoffset, xoffset + (int) (q2 * scale), yoffset + bpHeight);
		// q3 line
		g2d.drawLine(xoffset + (int) (q3 * scale), yoffset, xoffset + (int) (q3 * scale), yoffset + bpHeight);
		// top
		g2d.drawLine(xoffset + (int) (q1 * scale), yoffset, xoffset + (int) (q3 * scale), yoffset);
		// bottom
		g2d.drawLine(xoffset + (int) (q1 * scale), yoffset + bpHeight, xoffset + (int) (q3 * scale),
				yoffset + bpHeight);

		// left
		g2d.drawLine(xoffset + (int) (low * scale), (int) (bpHeight * 0.25) + yoffset, xoffset + (int) (low * scale),
				(int) (bpHeight * 0.75) + yoffset);
		// left line
		g2d.drawLine(xoffset + (int) (low * scale), (int) (bpHeight * 0.5) + yoffset, xoffset + (int) (q1 * scale),
				(int) (bpHeight * 0.5) + yoffset);
		// right
		g2d.drawLine(xoffset + (int) (high * scale), (int) (bpHeight * 0.25) + yoffset, xoffset + (int) (high * scale),
				(int) (bpHeight * 0.75) + yoffset);
		// right line
		g2d.drawLine(xoffset + (int) (q3 * scale), (int) (bpHeight * 0.5) + yoffset, xoffset + (int) (high * scale),
				(int) (bpHeight * 0.5) + yoffset);

		// outliers
		for (double outlier : outliers)
		{
			g2d.drawOval(xoffset + (int) (outlier * scale), (int) (bpHeight * 0.5) + yoffset - 1, 2, 2);
		}
	}
}

class SVGGraphics extends SVGGraphics2D
{
	public SVGGraphics(Document domFactory)
	{
		super(domFactory);
	}

	public SVGGraphics(Document domFactory, ImageHandler imageHandler, ExtensionHandler extensionHandler,
			boolean textAsShapes)
	{
		super(domFactory, imageHandler, extensionHandler, textAsShapes);
	}

	public SVGGraphics(SVGGeneratorContext generatorCtx, boolean textAsShapes)
	{
		super(generatorCtx, textAsShapes);
	}

	public SVGGraphics(SVGGraphics2D g)
	{
		super(g);
	}

	public DOMGroupManager domGroupManager()
	{
		return domGroupManager;
	}

	public SVGGeneratorContext generatorCtx()
	{
		return generatorCtx;
	}
}
```

### MOA with ARFF file
```
import java.util.Date;

import org.jfree.chart.ChartFactory;
import org.jfree.chart.ChartPanel;
import org.jfree.chart.JFreeChart;
import org.jfree.chart.plot.CategoryPlot;
import org.jfree.chart.plot.PlotOrientation;
import org.jfree.data.category.DefaultCategoryDataset;
import org.jfree.ui.ApplicationFrame;
import org.jfree.ui.RefineryUtilities;

import moa.classifiers.Classifier;
import moa.classifiers.meta.AdaptiveRandomForest;
import moa.classifiers.meta.LeveragingBag;
import moa.classifiers.meta.OzaBagAdwin;
import moa.classifiers.meta.PairedLearners;
import moa.classifiers.meta.WEKAClassifier;
import moa.evaluation.LearningCurve;
import moa.streams.ArffFileStream;
import moa.tasks.EvaluatePrequential;

public class MainTwo extends ApplicationFrame
{
	private static final long serialVersionUID = 1L;

	ChartPanel chartPanel = null;
	JFreeChart jfreechart = null;
	boolean withoutLegend = true;
	boolean tooltips = true;
	boolean urls = false;
	int height = 400;
	int width = 1600;

	public MainTwo(String title)
	{
		super(title);
		doIt();

		jfreechart = ChartFactory.createLineChart(title, "Record",
				"Accuracy Rate", this.createDataset(),
				PlotOrientation.VERTICAL, withoutLegend, tooltips, urls);
		jfreechart.getPlot().setBackgroundPaint(java.awt.Color.WHITE);;
		((CategoryPlot) jfreechart.getPlot()).getRangeAxis().setRange(min, max);

		chartPanel = new ChartPanel(jfreechart);
		// chartPanel.setBackground( java.awt.Color.WHITE );
		chartPanel.setPreferredSize(new java.awt.Dimension(width, height));
		this.setContentPane(chartPanel);
		this.pack();
		RefineryUtilities.centerFrameOnScreen(this);
		this.setVisible(true);
	}

	private void refreshChart(String title)
	{
		doIt();

		chartPanel.removeAll();
		chartPanel.revalidate(); // This removes the old chart
		jfreechart = ChartFactory.createLineChart(title, "Record",
				"Accuracy Rate", this.createDataset(),
				PlotOrientation.VERTICAL, withoutLegend, tooltips, urls);
		jfreechart.getPlot().setBackgroundPaint(java.awt.Color.WHITE);
		((CategoryPlot) jfreechart.getPlot()).getRangeAxis().setRange(min, max);
		chartPanel = new ChartPanel(jfreechart);
		chartPanel.setPreferredSize(new java.awt.Dimension(width, height));
		// chartPanel.add(chartPanel);
		chartPanel.repaint(); // This method makes the new chart appear
		this.setContentPane(chartPanel);
		this.pack();
		RefineryUtilities.centerFrameOnScreen(this);
		this.setVisible(true);

	}

	private static final String DEFAULT_INPUT_FILE = "data/iris_stream.arff";

	public static void main(String[] args) throws InterruptedException
	{
		MainTwo myObj = new MainTwo(DEFAULT_INPUT_FILE + ": results initialed on "
				+ new Date());
		long count = 0;
		while (true)
		{
			Thread.sleep(4000);
			count++;
			myObj.refreshChart(DEFAULT_INPUT_FILE + ": results reloaded on "
					+ new Date());
			System.out.println("refreshing " + count);
		}
	}

	// set EvaluatePrequential's parameter
	int maxInstances = 1000000;
	int timeLimit = -1;
	int sampleFrequencyOption = 5;
	
	private void doIt()
	{
		// prepare input file for streaming evaluation
		String arffFilePath = System.getenv("HOME") + "/" + DEFAULT_INPUT_FILE;
		arffFilePath = DEFAULT_INPUT_FILE;
		ArffFileStream myArffstream = null;
		try
		{
			myArffstream = new ArffFileStream(arffFilePath, -1);
			myArffstream.prepareForUse();
		}
		catch (Exception e)
		{
			System.out
					.println("Problem with loading arff file. Quit the program");
			System.exit(-1);
		}
		
		double[][] output = null;
		Classifier targetClasifier = null;
		
		targetClasifier = new PairedLearners();
		output = doWork(targetClasifier, myArffstream);
		xValLp = output[0];
		accValLp = output[1];

		targetClasifier = new LeveragingBag();
		output = doWork(targetClasifier, myArffstream);
		xValLb = output[0];
		accValLb = output[1];
		
		targetClasifier = new WEKAClassifier();
		output = doWork(targetClasifier, myArffstream);
		xValNbu = output[0];
		accValNbu = output[1];
		
		targetClasifier = new AdaptiveRandomForest();
		output = doWork(targetClasifier, myArffstream);
		xValArf = output[0];
		accValArf = output[1];
		
		targetClasifier = new OzaBagAdwin();
		output = doWork(targetClasifier, myArffstream);
		xValBa = output[0];
		accValBa = output[1];
		
		System.out.println("found records in " + DEFAULT_INPUT_FILE
				+ ": nearly " + xValLp.length * sampleFrequencyOption
				+ ", " + xValLb.length * sampleFrequencyOption
				+ ", " + xValNbu.length * sampleFrequencyOption
				+ ", " + xValArf.length * sampleFrequencyOption
				+ ", " + xValBa.length * sampleFrequencyOption
				);
		
	}
	
	private double[][] doWork(Classifier targetClasifier, ArffFileStream myArffstream )
	{
		// do the learning and checking using evaluate-prequential technique
		EvaluatePrequential ep = new EvaluatePrequential();
		ep.instanceLimitOption.setValue(maxInstances);
		ep.learnerOption.setCurrentObject(targetClasifier);
		ep.streamOption.setCurrentObject(myArffstream);
		ep.sampleFrequencyOption.setValue(sampleFrequencyOption);
		ep.timeLimitOption.setValue(timeLimit);
		ep.prepareForUse();

		// do the task and get the result
		LearningCurve le = (LearningCurve) ep.doTask();
		// System.out.println("Evaluate prequential using targetClasifier");
		// System.out.println(le);

		int size = le.numEntries();
		double[] xValTemp = new double[size];
		double[] accValTemp = new double[size];
		for (int i = 0; i < size; i++)
		{
			xValTemp[i] = le.getMeasurement(i, 0);
			accValTemp[i] = le.getMeasurement(i, 4);
		}
		double[][] result = new double[2][];
		result[0] = xValTemp;
		result[1] = accValTemp;
		return result;
	}

	double[] xValLp = null;
	double[] xValLb = null;
	double[] xValNbu = null;
	double[] xValArf = null;
	double[] xValBa = null;
	double[] accValLp = null;
	double[] accValLb = null;
	double[] accValNbu = null;
	double[] accValArf = null;
	double[] accValBa = null;
	double min = 99.9999;
	double max = 100.0;

	private DefaultCategoryDataset createDataset()
	{
		DefaultCategoryDataset dataset = new DefaultCategoryDataset();

		for (int i = 0; i < xValLp.length; i++)
		{
			if (this.accValLp[i] > max)
				this.max = this.accValLp[i];
			if (this.accValLp[i] < min)
				this.min = this.accValLp[i];
			dataset.addValue(this.accValLp[i], "PairedLearners", this.xValLp[i] + "");
		}
		
		for (int i = 0; i < xValLb.length; i++)
		{
			if (this.accValLb[i] > max)
				this.max = this.accValLb[i];
			if (this.accValLb[i] < min)
				this.min = this.accValLb[i];
			dataset.addValue(this.accValLb[i], "LeveragingBag", this.xValLb[i] + "");
		}
		
		for (int i = 0; i < xValNbu.length; i++)
		{
			if (this.accValNbu[i] > max)
				this.max = this.accValNbu[i];
			if (this.accValNbu[i] < min)
				this.min = this.accValNbu[i];
			dataset.addValue(this.accValNbu[i], "NaiveBayesUpdateable", this.xValNbu[i] + "");
		}
		
		for (int i = 0; i < xValArf.length; i++)
		{
			if (this.accValArf[i] > max)
				this.max = this.accValArf[i];
			if (this.accValArf[i] < min)
				this.min = this.accValArf[i];
			dataset.addValue(this.accValArf[i], "AdaptiveRandomForest", this.xValArf[i] + "");
		}
		
		for (int i = 0; i < xValBa.length; i++)
		{
			if (this.accValBa[i] > max)
				this.max = this.accValBa[i];
			if (this.accValBa[i] < min)
				this.min = this.accValBa[i];
			dataset.addValue(this.accValBa[i], "OzaBagAdwin", this.xValBa[i] + "");
		}
		return dataset;
	}

}

```
Streaming data into arff file in Linux
```
cat iris_head.arff > iris_stream.arff;
cat iris_data.arff | awk 'BEGIN {srand()} !/^$/ { if (rand() <= .01) print $0}' >> iris_stream.arff;
cat iris_data.arff | awk 'BEGIN {srand()} !/^$/ { if (rand() <= .01) print $0}' >> iris_stream.arff;
cat iris_data.arff | awk 'BEGIN {srand()} !/^$/ { if (rand() <= .01) print $0}' >> iris_stream.arff;
sleep 10;
while true;
do 
cat iris_data.arff | awk 'BEGIN {srand()} !/^$/ { if (rand() <= .01) print $0}' >> iris_stream.arff
cat iris_data.arff | awk 'BEGIN {srand()} !/^$/ { if (rand() <= .01) print $0}' >> iris_stream.arff
cat iris_data.arff | awk 'BEGIN {srand()} !/^$/ { if (rand() <= .01) print $0}' >> iris_stream.arff;
sleep 2;
done
```
