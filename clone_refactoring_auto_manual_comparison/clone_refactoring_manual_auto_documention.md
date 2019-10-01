# Documentation on Manual Refactoring experiments and Automatic Refactoring

### commons_math_3_6_1

## Automatic Refactoring

317	src/test/java	org.apache.commons.math3.geometry.euclidean.threed	FieldRotationDSTest	testComposeVectorOperator	()V	809	836

317	src/test/java	org.apache.commons.math3.geometry.euclidean.threed	FieldRotationDSTest	testComposeFrameTransform	()V	838	865


// Original Code

```java

@Test
public void testComposeVectorOperator() throws MathIllegalArgumentException {

    FieldRotation<DerivativeStructure> r1       = new FieldRotation<DerivativeStructure>(createVector(2, -3, 5),
                                                                                         createAngle(1.7),
                                                                                         RotationConvention.VECTOR_OPERATOR);
    FieldRotation<DerivativeStructure> r2       = new FieldRotation<DerivativeStructure>(createVector(-1, 3, 2),
                                                                                         createAngle(0.3),
                                                                                         RotationConvention.VECTOR_OPERATOR);
    FieldRotation<DerivativeStructure> r3       = r2.compose(r1, RotationConvention.VECTOR_OPERATOR);
    FieldRotation<DerivativeStructure> r3Double = r2.compose(new Rotation(r1.getQ0().getReal(),
                                                                          r1.getQ1().getReal(),
                                                                          r1.getQ2().getReal(),
                                                                          r1.getQ3().getReal(),
                                                                          false),
                                                             RotationConvention.VECTOR_OPERATOR);

    for (double x = -0.9; x < 0.9; x += 0.2) {
        for (double y = -0.9; y < 0.9; y += 0.2) {
            for (double z = -0.9; z < 0.9; z += 0.2) {
                FieldVector3D<DerivativeStructure> u = createVector(x, y, z);
                checkVector(r2.applyTo(r1.applyTo(u)), r3.applyTo(u));
                checkVector(r2.applyTo(r1.applyTo(u)), r3Double.applyTo(u));
            }
        }
    }

}

@Test
public void testComposeFrameTransform() throws MathIllegalArgumentException {

    FieldRotation<DerivativeStructure> r1       = new FieldRotation<DerivativeStructure>(createVector(2, -3, 5),
                                                                                         createAngle(1.7),
                                                                                         RotationConvention.FRAME_TRANSFORM);
    FieldRotation<DerivativeStructure> r2       = new FieldRotation<DerivativeStructure>(createVector(-1, 3, 2),
                                                                                         createAngle(0.3),
                                                                                         RotationConvention.FRAME_TRANSFORM);
    FieldRotation<DerivativeStructure> r3       = r2.compose(r1, RotationConvention.FRAME_TRANSFORM);
    FieldRotation<DerivativeStructure> r3Double = r2.compose(new Rotation(r1.getQ0().getReal(),
                                                                          r1.getQ1().getReal(),
                                                                          r1.getQ2().getReal(),
                                                                          r1.getQ3().getReal(),
                                                                          false),
                                                             RotationConvention.FRAME_TRANSFORM);

    for (double x = -0.9; x < 0.9; x += 0.2) {
        for (double y = -0.9; y < 0.9; y += 0.2) {
            for (double z = -0.9; z < 0.9; z += 0.2) {
                FieldVector3D<DerivativeStructure> u = createVector(x, y, z);
                checkVector(r1.applyTo(r2.applyTo(u)), r3.applyTo(u));
                checkVector(r1.applyTo(r2.applyTo(u)), r3Double.applyTo(u));
            }
        }
    }

}

```

// Refactored

```java

    @Test
	public void testComposeVectorOperator() throws Exception {
		this.fieldRotationDSTestTestComposeTemplate(new FieldRotationDSTestTestComposeVectorOperatorAdapterImpl(),
				RotationConvention.VECTOR_OPERATOR, RotationConvention.VECTOR_OPERATOR,
				RotationConvention.VECTOR_OPERATOR, RotationConvention.VECTOR_OPERATOR);
	}

    @Test
	public void testComposeFrameTransform() throws Exception {
		this.fieldRotationDSTestTestComposeTemplate(new FieldRotationDSTestTestComposeFrameTransformAdapterImpl(),
				RotationConvention.FRAME_TRANSFORM, RotationConvention.FRAME_TRANSFORM,
				RotationConvention.FRAME_TRANSFORM, RotationConvention.FRAME_TRANSFORM);
	}
	
	
	public void fieldRotationDSTestTestComposeTemplate(FieldRotationDSTestTestComposeAdapter adapter,
			RotationConvention rotationConvention1, RotationConvention rotationConvention2,
			RotationConvention rotationConvention3, RotationConvention rotationConvention4) throws Exception {
		FieldRotation<DerivativeStructure> r1 = new FieldRotation<DerivativeStructure>(createVector(2, -3, 5),
				createAngle(1.7), rotationConvention1);
		FieldRotation<DerivativeStructure> r2 = new FieldRotation<DerivativeStructure>(createVector(-1, 3, 2),
				createAngle(0.3), rotationConvention2);
		FieldRotation<DerivativeStructure> r3 = r2.compose(r1, rotationConvention3);
		FieldRotation<DerivativeStructure> r3Double = r2.compose(new Rotation(r1.getQ0().getReal(),
				r1.getQ1().getReal(), r1.getQ2().getReal(), r1.getQ3().getReal(), false), rotationConvention4);
		for (double x = -0.9; x < 0.9; x += 0.2) {
			for (double y = -0.9; y < 0.9; y += 0.2) {
				for (double z = -0.9; z < 0.9; z += 0.2) {
					FieldVector3D<DerivativeStructure> u = createVector(x, y, z);
					checkVector(adapter.action2(r2, r1).applyTo(adapter.action1(r1, r2).applyTo(u)), r3.applyTo(u));
					checkVector(adapter.action4(r2, r1).applyTo(adapter.action3(r1, r2).applyTo(u)),
							r3Double.applyTo(u));
				}
			}
		}
	}

	interface FieldRotationDSTestTestComposeAdapter {
		FieldRotation<DerivativeStructure> action1(FieldRotation<DerivativeStructure> fieldRotationDerivativeStructure1,
				FieldRotation<DerivativeStructure> fieldRotationDerivativeStructure2);

		FieldRotation<DerivativeStructure> action2(FieldRotation<DerivativeStructure> fieldRotationDerivativeStructure1,
				FieldRotation<DerivativeStructure> fieldRotationDerivativeStructure2);

		FieldRotation<DerivativeStructure> action3(FieldRotation<DerivativeStructure> fieldRotationDerivativeStructure1,
				FieldRotation<DerivativeStructure> fieldRotationDerivativeStructure2);

		FieldRotation<DerivativeStructure> action4(FieldRotation<DerivativeStructure> fieldRotationDerivativeStructure1,
				FieldRotation<DerivativeStructure> fieldRotationDerivativeStructure2);
	}

	class FieldRotationDSTestTestComposeVectorOperatorAdapterImpl implements FieldRotationDSTestTestComposeAdapter {
		public FieldRotation<DerivativeStructure> action1(
				FieldRotation<DerivativeStructure> fieldRotationDerivativeStructure1,
				FieldRotation<DerivativeStructure> fieldRotationDerivativeStructure2) {
			return fieldRotationDerivativeStructure1;
		}

		public FieldRotation<DerivativeStructure> action2(
				FieldRotation<DerivativeStructure> fieldRotationDerivativeStructure1,
				FieldRotation<DerivativeStructure> fieldRotationDerivativeStructure2) {
			return fieldRotationDerivativeStructure1;
		}

		public FieldRotation<DerivativeStructure> action3(
				FieldRotation<DerivativeStructure> fieldRotationDerivativeStructure1,
				FieldRotation<DerivativeStructure> fieldRotationDerivativeStructure2) {
			return fieldRotationDerivativeStructure1;
		}

		public FieldRotation<DerivativeStructure> action4(
				FieldRotation<DerivativeStructure> fieldRotationDerivativeStructure1,
				FieldRotation<DerivativeStructure> fieldRotationDerivativeStructure2) {
			return fieldRotationDerivativeStructure1;
		}
	}

	class FieldRotationDSTestTestComposeFrameTransformAdapterImpl implements FieldRotationDSTestTestComposeAdapter {
		public FieldRotation<DerivativeStructure> action1(
				FieldRotation<DerivativeStructure> fieldRotationDerivativeStructure1,
				FieldRotation<DerivativeStructure> fieldRotationDerivativeStructure2) {
			return fieldRotationDerivativeStructure2;
		}

		public FieldRotation<DerivativeStructure> action2(
				FieldRotation<DerivativeStructure> fieldRotationDerivativeStructure1,
				FieldRotation<DerivativeStructure> fieldRotationDerivativeStructure2) {
			return fieldRotationDerivativeStructure2;
		}

		public FieldRotation<DerivativeStructure> action3(
				FieldRotation<DerivativeStructure> fieldRotationDerivativeStructure1,
				FieldRotation<DerivativeStructure> fieldRotationDerivativeStructure2) {
			return fieldRotationDerivativeStructure2;
		}

		public FieldRotation<DerivativeStructure> action4(
				FieldRotation<DerivativeStructure> fieldRotationDerivativeStructure1,
				FieldRotation<DerivativeStructure> fieldRotationDerivativeStructure2) {
			return fieldRotationDerivativeStructure2;
		}
	}
	
```


### jodatime-2.10

## Automatic Refactoring

359	src/test/java	org.joda.time	TestInstant_Basics	testGet_DateTimeField	()V	145	173						2

359	src/test/java	org.joda.time	TestMutableDateTime_Basics	testGet_DateTimeField	()V	121	149

```java

//refactored: TestInstant_Basics	testGet_DateTimeField
public void testGet_DateTimeField() throws Exception {
  TestBasicsTestGet_DateTimeFieldTemplate.testBasicsTestGet_DateTimeFieldTemplate(Instant.class);
}


//refactored: TestMutableDateTime_Basics	testGet_DateTimeField
public void testGet_DateTimeField() throws Exception {
  TestBasicsTestGet_DateTimeFieldTemplate.testBasicsTestGet_DateTimeFieldTemplate(MutableDateTime.class);
}

// testBasicsTestGet_DateTimeFieldTemplate.java
package org.joda.time;
import org.joda.time.base.AbstractInstant;
import static junit.framework.Assert.assertEquals;
import org.joda.time.chrono.ISOChronology;
import static org.joda.time.chrono.ISOChronology.getInstance;
import static junit.framework.Assert.fail;
public class TestBasicsTestGet_DateTimeFieldTemplate {
  public static <TInstant extends AbstractInstant>void testBasicsTestGet_DateTimeFieldTemplate(  Class<TInstant> clazzTInstant) throws Exception {
    TInstant test=clazzTInstant.newInstance();
    assertEquals(1,test.get(ISOChronology.getInstance().era()));
    assertEquals(20,test.get(ISOChronology.getInstance().centuryOfEra()));
    assertEquals(2,test.get(ISOChronology.getInstance().yearOfCentury()));
    assertEquals(2002,test.get(ISOChronology.getInstance().yearOfEra()));
    assertEquals(2002,test.get(ISOChronology.getInstance().year()));
    assertEquals(6,test.get(ISOChronology.getInstance().monthOfYear()));
    assertEquals(9,test.get(ISOChronology.getInstance().dayOfMonth()));
    assertEquals(2002,test.get(ISOChronology.getInstance().weekyear()));
    assertEquals(23,test.get(ISOChronology.getInstance().weekOfWeekyear()));
    assertEquals(7,test.get(ISOChronology.getInstance().dayOfWeek()));
    assertEquals(160,test.get(ISOChronology.getInstance().dayOfYear()));
    assertEquals(0,test.get(ISOChronology.getInstance().halfdayOfDay()));
    assertEquals(1,test.get(ISOChronology.getInstance().hourOfHalfday()));
    assertEquals(1,test.get(ISOChronology.getInstance().clockhourOfDay()));
    assertEquals(1,test.get(ISOChronology.getInstance().clockhourOfHalfday()));
    assertEquals(1,test.get(ISOChronology.getInstance().hourOfDay()));
    assertEquals(0,test.get(ISOChronology.getInstance().minuteOfHour()));
    assertEquals(60,test.get(ISOChronology.getInstance().minuteOfDay()));
    assertEquals(0,test.get(ISOChronology.getInstance().secondOfMinute()));
    assertEquals(60 * 60,test.get(ISOChronology.getInstance().secondOfDay()));
    assertEquals(0,test.get(ISOChronology.getInstance().millisOfSecond()));
    assertEquals(60 * 60 * 1000,test.get(ISOChronology.getInstance().millisOfDay()));
    try {
      test.get((DateTimeField)null);
      fail();
    }
 catch (    IllegalArgumentException ex) {
    }
  }
}

```

89	src/test/java	org.joda.time.format	TestPeriodFormatterBuilder	testFormatPrefixSimple1	()V	314	324						2

89	src/test/java	org.joda.time.format	TestPeriodFormatterBuilder	testFormatPrefixSimple2	()V	326	336

```java

// TestPeriodFormatterBuilder	testFormatPrefixSimple1
public void testFormatPrefixSimple1() {
this.testPeriodFormatterBuilderTestFormatPrefixTemplate(
    new TestPeriodFormatterBuilderTestFormatPrefixSimple1AdapterImpl(), "Years:", "Years:1", "Years:0");
}

// TestPeriodFormatterBuilder	testFormatPrefixSimple2
public void testFormatPrefixSimple2() {
this.testPeriodFormatterBuilderTestFormatPrefixTemplate(
    new TestPeriodFormatterBuilderTestFormatPrefixSimple2AdapterImpl(), "Hours:", "Hours:5", "Hours:0");
}


//Template, Adapter, and Impl
public void testPeriodFormatterBuilderTestFormatPrefixTemplate(
    TestPeriodFormatterBuilderTestFormatPrefixAdapter adapter, String string1, String string2, String string3) {
  PeriodFormatter f = adapter.append(builder.appendPrefix(string1)).toFormatter();
  assertEquals(string2, f.print(PERIOD));
  assertEquals(7, f.getPrinter().calculatePrintedLength(PERIOD, null));
  assertEquals(1, f.getPrinter().countFieldsToPrint(PERIOD, Integer.MAX_VALUE, null));
  Period p = new Period(0, 0, 0, 0, 0, 0, 0, 0);
  assertEquals(string3, f.print(p));
  assertEquals(7, f.getPrinter().calculatePrintedLength(p, null));
  assertEquals(1, f.getPrinter().countFieldsToPrint(p, Integer.MAX_VALUE, null));
}

interface TestPeriodFormatterBuilderTestFormatPrefixAdapter {
  PeriodFormatterBuilder append(PeriodFormatterBuilder periodFormatterBuilder1);
}

class TestPeriodFormatterBuilderTestFormatPrefixSimple1AdapterImpl
    implements TestPeriodFormatterBuilderTestFormatPrefixAdapter {
  public PeriodFormatterBuilder append(PeriodFormatterBuilder periodFormatterBuilder1) {
    return periodFormatterBuilder1.appendYears();
  }
}

class TestPeriodFormatterBuilderTestFormatPrefixSimple2AdapterImpl
    implements TestPeriodFormatterBuilderTestFormatPrefixAdapter {
  public PeriodFormatterBuilder append(PeriodFormatterBuilder periodFormatterBuilder1) {
    return periodFormatterBuilder1.appendHours();
  }
}
```


1337	src/test/java	org.joda.time	TestInstant_Basics	testIsEqual_RI	()V	262	285						2

1337	src/test/java	org.joda.time	TestInstant_Basics	testIsBefore_RI	()V	300	323

// Original Code

```java

public void testIsEqual_RI() {
    Instant test1 = new Instant(TEST_TIME1);
    Instant test1a = new Instant(TEST_TIME1);
    assertEquals(true, test1.isEqual(test1a));
    assertEquals(true, test1a.isEqual(test1));
    assertEquals(true, test1.isEqual(test1));
    assertEquals(true, test1a.isEqual(test1a));

    Instant test2 = new Instant(TEST_TIME2);
    assertEquals(false, test1.isEqual(test2));
    assertEquals(false, test2.isEqual(test1));

    DateTime test3 = new DateTime(TEST_TIME2, GregorianChronology.getInstance(PARIS));
    assertEquals(false, test1.isEqual(test3));
    assertEquals(false, test3.isEqual(test1));
    assertEquals(true, test3.isEqual(test2));

    assertEquals(false, test2.isEqual(new MockInstant()));
    assertEquals(true, test1.isEqual(new MockInstant()));

    assertEquals(false, new Instant(TEST_TIME_NOW + 1).isEqual(null));
    assertEquals(true, new Instant(TEST_TIME_NOW).isEqual(null));
    assertEquals(false, new Instant(TEST_TIME_NOW - 1).isEqual(null));
}

public void testIsBefore_RI() {
    Instant test1 = new Instant(TEST_TIME1);
    Instant test1a = new Instant(TEST_TIME1);
    assertEquals(false, test1.isBefore(test1a));
    assertEquals(false, test1a.isBefore(test1));
    assertEquals(false, test1.isBefore(test1));
    assertEquals(false, test1a.isBefore(test1a));

    Instant test2 = new Instant(TEST_TIME2);
    assertEquals(true, test1.isBefore(test2));
    assertEquals(false, test2.isBefore(test1));

    DateTime test3 = new DateTime(TEST_TIME2, GregorianChronology.getInstance(PARIS));
    assertEquals(true, test1.isBefore(test3));
    assertEquals(false, test3.isBefore(test1));
    assertEquals(false, test3.isBefore(test2));

    assertEquals(false, test2.isBefore(new MockInstant()));
    assertEquals(false, test1.isBefore(new MockInstant()));

    assertEquals(false, new Instant(TEST_TIME_NOW + 1).isBefore(null));
    assertEquals(false, new Instant(TEST_TIME_NOW).isBefore(null));
    assertEquals(true, new Instant(TEST_TIME_NOW - 1).isBefore(null));
}


```

// Refactored

```java
// TestInstant_Basics	testIsEqual_RI
public void testIsEqual_RI() {
this.testInstant_BasicsTestIsRITemplate(new TestInstant_BasicsTestIsEqual_RIAdapterImpl(), true, true, true,
    true, false, false, true, true, true, false);
}

// TestInstant_Basics	testIsBefore_RI
public void testIsBefore_RI() {
this.testInstant_BasicsTestIsRITemplate(new TestInstant_BasicsTestIsBefore_RIAdapterImpl(), false, false, false,
    false, true, true, false, false, false, true);
}

//Template, Adapter, and Impl
public void testInstant_BasicsTestIsRITemplate(TestInstant_BasicsTestIsRIAdapter adapter, boolean b1, boolean b2,
    boolean b3, boolean b4, boolean b5, boolean b6, boolean b7, boolean b8, boolean b9, boolean b10) {
  Instant test1 = new Instant(TEST_TIME1);
  Instant test1a = new Instant(TEST_TIME1);
  assertEquals(b1, adapter.is(test1, test1a));
  assertEquals(b2, adapter.is(test1a, test1));
  assertEquals(b3, adapter.is(test1, test1));
  assertEquals(b4, adapter.is(test1a, test1a));
  Instant test2 = new Instant(TEST_TIME2);
  assertEquals(b5, adapter.is(test1, test2));
  assertEquals(false, adapter.is(test2, test1));
  DateTime test3 = new DateTime(TEST_TIME2, GregorianChronology.getInstance(PARIS));
  assertEquals(b6, adapter.is(test1, test3));
  assertEquals(false, adapter.is1(test3, test1));
  assertEquals(b7, adapter.is1(test3, test2));
  assertEquals(false, adapter.is(test2, new MockInstant()));
  assertEquals(b8, adapter.is(test1, new MockInstant()));
  assertEquals(false, adapter.is(new Instant(TEST_TIME_NOW + 1), null));
  assertEquals(b9, adapter.is(new Instant(TEST_TIME_NOW), null));
  assertEquals(b10, adapter.is(new Instant(TEST_TIME_NOW - 1), null));
}

interface TestInstant_BasicsTestIsRIAdapter {
  boolean is(Instant instant1, ReadableInstant readableInstant1);

  boolean is1(DateTime dateTime1, ReadableInstant readableInstant1);
}

class TestInstant_BasicsTestIsEqual_RIAdapterImpl implements TestInstant_BasicsTestIsRIAdapter {
  public boolean is(Instant test1, ReadableInstant test1a) {
    return test1.isEqual(test1a);
  }

  public boolean is1(DateTime test3, ReadableInstant test1) {
    return test3.isEqual(test1);
  }
}

class TestInstant_BasicsTestIsBefore_RIAdapterImpl implements TestInstant_BasicsTestIsRIAdapter {
  public boolean is(Instant test1, ReadableInstant test1a) {
    return test1.isBefore(test1a);
  }

  public boolean is1(DateTime test3, ReadableInstant test1) {
    return test3.isBefore(test1);
  }
}
```

### jfreechart-1.0.10-original

## Manual Refactoring Attempt

// 8	tests	org.jfree.chart.axis.junit	CategoryAxis3DTests	testCloning	()V	82	94

// 8	tests	org.jfree.chart.axis.junit	CategoryAxisTests	testCloning	()V	173	185	

// 8	tests	org.jfree.chart.axis.junit	ColorBarTests	testCloning	()V	116	128

// 8	tests	org.jfree.chart.axis.junit	SubCategoryAxisTests	testCloning	()V	127	139
 
 
The *testCloning* test case in **ColorBarTests** cannot be refactored automatically (by the current refactoring rule), 
because **CategoryAxis**, **CateGoryAxis3D**, and **SubCategoryAxis** are all subclasses of the type **CategoryAxis**, but **ColorBar** is of its own stream. 
A "PDG gap node" is reported in this scenario.


```java

// CategoryAxis3DTests	testCloning 

	public void testCloning() throws Exception {
		CategoryTestsTestCloningTemplate.categoryTestsTestCloningTemplate(
				new CategoryAxis3DTestsTestCloningAdapterImpl(), CategoryAxis3D.class);
	}

	class CategoryAxis3DTestsTestCloningAdapterImpl implements CategoryTestsTestCloningAdapter<CategoryAxis3D> {
		public Class<? extends CategoryAxis3D> getClass(CategoryAxis3D a1) {
			return a1.getClass();
		}
	}
	
// CategoryAxisTests	testCloning

	public void testCloning() throws Exception {
		CategoryTestsTestCloningTemplate.categoryTestsTestCloningTemplate(new CategoryAxisTestsTestCloningAdapterImpl(),
				CategoryAxis.class);
	}
	
	class CategoryAxisTestsTestCloningAdapterImpl implements CategoryTestsTestCloningAdapter<CategoryAxis> {
		public Class<? extends CategoryAxis> getClass(CategoryAxis a1) {
			return a1.getClass();
		}
	}
	

// SubCategoryAxisTests	testCloning

	public void testCloning() throws Exception {
		CategoryTestsTestCloningTemplate.categoryTestsTestCloningTemplate(
				new SubCategoryAxisTestsTestCloningAdapterImpl(), SubCategoryAxis.class);
	}
	
	class SubCategoryAxisTestsTestCloningAdapterImpl implements CategoryTestsTestCloningAdapter<SubCategoryAxis> {
		public Class<? extends SubCategoryAxis> getClass(SubCategoryAxis a1) {
			return a1.getClass();
		}
	}	
		
// Template

package org.jfree.chart.axis.junit;
import org.jfree.chart.axis.CategoryAxis;
import java.lang.String;
import java.lang.System;
import static junit.framework.Assert.assertTrue;
public class CategoryTestsTestCloningTemplate {
  public static <TCategory extends CategoryAxis>void categoryTestsTestCloningTemplate(  CategoryTestsTestCloningAdapter<TCategory> adapter,  Class<TCategory> clazzTCategory) throws Exception {
    TCategory a1=clazzTCategory.getDeclaredConstructor(String.class).newInstance("Test");
    TCategory a2=null;
    try {
      a2=(TCategory)a1.clone();
    }
 catch (    CloneNotSupportedException e) {
      System.err.println("Failed to clone.");
    }
    assertTrue(a1 != a2);
    assertTrue(adapter.getClass(a1) == adapter.getClass(a2));
    assertTrue(a1.equals(a2));
  }
}

interface CategoryTestsTestCloningAdapter<TCategory> {
	Class<? extends TCategory> getClass(TCategory tCategory1);
}

```

