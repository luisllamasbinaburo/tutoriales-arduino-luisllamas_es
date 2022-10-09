> Códigos de ejemplo de los tutoriales de www.luisllamas.es
>
> Enlace entrada: https://www.luisllamas.es/resolver-robots-articulados-con-arduino/
>
> Todo el contenido distribuido bajo licencia CCC, salvo indicación expresa

```cpp
float L1, L2, D;
float AbsoluteAngle1, AbsoluteAngle2;
float RelativeAngle12;
float TargetX, TargetY;

float Pythagoras(const float& x, const float& y)
{
	const float d = sqrt(x * x + y * y);
	return d;
}

float SolveInverseCosine(const float& l1, const float& l2, const float& l3)
{
	const float angle3 = acos((l1 * l1 + l2 * l2 - l3 * l3) / (2 * l1 * l2));

	return angle3;
}

float AbsoluteToRelative(float absolute1, float absolute2)
{
	return absolute2 - absolute1 + PI;
}

float RelativeToAbsolute(float relative12, float absolute1)
{
	return absolute1 + relative12 - PI;
}

void SolveTriangle(float& x, float& y, float& l1, float& l2, float& d, float& absoluteAngle1, float& absoluteAngle2, float& relativeAngle12)
{
	const float angle_0 = atan2(y, x);
	d = Pythagoras(x, y);

	const float angle_l1 = SolveInverseCosine(d, l2, l1);
	const float angle_l2 = SolveInverseCosine(d, l1, l2);
	const float angle_D = PI - angle_l1 - angle_l2;

	absoluteAngle1 = angle_0 + angle_l2;
	relativeAngle12 = angle_D;
	absoluteAngle2 = RelativeToAbsolute(relativeAngle12, absoluteAngle1);
}

void SolveDirect(float angle1, float angle2)
{
	AbsoluteAngle1 = angle1;
	AbsoluteAngle2 = angle2;

	SolveDirect();
}

void SolveDirect()
{
	RelativeAngle12 = AbsoluteToRelative(AbsoluteAngle1, AbsoluteAngle2);

	TargetX = L1 * cos(AbsoluteAngle1) + L2 * cos(AbsoluteAngle2);
	TargetY = L1 * sin(AbsoluteAngle1) + L2 * sin(AbsoluteAngle2);
	D = Pythagoras(TargetX, TargetY);
}

void SolveReverse()
{
	D = Pythagoras(TargetX, TargetY);

	float P2x = TargetX;
	float P2y = TargetY;

	float D0;
	SolveTriangle(P2x, P2y, L1, L2, D0, AbsoluteAngle1, AbsoluteAngle2, RelativeAngle12);
}

void Debug()
{
	Serial.print(F("Target X:\t"));
	Serial.println(TargetX, 4);
	Serial.print(F("Target Y:\t"));
	Serial.println(TargetY, 4);

	Serial.print(F("Abs. Angle 1:\t"));
	Serial.println(degrees(AbsoluteAngle1), 4);
	Serial.print(F("Abs. Angle 2:\t"));
	Serial.println(degrees(AbsoluteAngle2), 4);

	Serial.print(F("Rel. Angle 12:\t"));
	Serial.println(degrees(RelativeAngle12), 4);
}


void setup()
{
	Serial.begin(115200);
	while (!Serial) { ; }

	L1 = 100; L2 = 50;

	Serial.println(F("Solve Direct Relative"));
	SolveDirect(radians(55), radians(-20));
	Debug();

	Serial.println(F("Solve Inverse"));
	TargetX = 104.3423;
	TargetY = 64.8141;
	SolveReverse();
	Debug();
}


void loop()
{
	// Do nothing
	delay(10000);
}
```

```cpp
float L1, L2, L3, D;
float AbsoluteAngle1, AbsoluteAngle2, AbsoluteAngle3;
float RelativeAngle12, RelativeAngle23;
float TargetX, TargetY;

float Pythagoras(const float& x, const float& y)
{
	const float d = sqrt(x * x + y * y);
	return d;
}

float SolveInverseCosine(const float& l1, const float& l2, const float& l3)
{
	const float angle3 = acos((l1 * l1 + l2 * l2 - l3 * l3) / (2 * l1 * l2));

	return angle3;
}

float AbsoluteToRelative(float absolute1, float absolute2)
{
	return absolute2 - absolute1 + PI;
}

float RelativeToAbsolute(float relative12, float absolute1)
{
	return absolute1 + relative12 - PI;
}

void SolveTriangle(float& x, float& y, float& l1, float& l2, float& d, float& absoluteAngle1, float& absoluteAngle2, float& relativeAngle12)
{
	const float angle_0 = atan2(y, x);
	d = Pythagoras(x, y);

	const float angle_l1 = SolveInverseCosine(d, l2, l1);
	const float angle_l2 = SolveInverseCosine(d, l1, l2);
	const float angle_D = PI - angle_l1 - angle_l2;

	absoluteAngle1 = angle_0 + angle_l2;
	relativeAngle12 = angle_D;
	absoluteAngle2 = RelativeToAbsolute(relativeAngle12, absoluteAngle1);
}

void SolveDirect(float angle1, float angle2, float angle3)
{
	AbsoluteAngle1 = angle1;
	AbsoluteAngle2 = angle2;
	AbsoluteAngle3 = angle3;

	SolveDirect();
}

void SolveDirect()
{
	RelativeAngle12 = AbsoluteToRelative(AbsoluteAngle1, AbsoluteAngle2);
	RelativeAngle23 = AbsoluteToRelative(AbsoluteAngle2, AbsoluteAngle3);

	TargetX = L1 * cos(AbsoluteAngle1) + L2 * cos(AbsoluteAngle2) + L3 * cos(AbsoluteAngle3);
	TargetY = L1 * sin(AbsoluteAngle1) + L2 * sin(AbsoluteAngle2) + L3 * sin(AbsoluteAngle3);
	D = Pythagoras(TargetX, TargetY);
}

void SolveReverse(float absoluteAngle3)
{
	AbsoluteAngle3 = absoluteAngle3;
	D = Pythagoras(TargetX, TargetY);

	float P2x = TargetX - L3 * cos(AbsoluteAngle3);
	float P2y = TargetY - L3 * sin(AbsoluteAngle3);

	float D0;
	SolveTriangle(P2x, P2y, L1, L2, D0, AbsoluteAngle1, AbsoluteAngle2, RelativeAngle12);

	RelativeAngle23 = AbsoluteToRelative(AbsoluteAngle2, AbsoluteAngle3);
}

void Debug()
{
	Serial.print(F("Target X:\t"));
	Serial.println(TargetX, 4);
	Serial.print(F("Target Y:\t"));
	Serial.println(TargetY, 4);

	Serial.print(F("Abs. Angle 1:\t"));
	Serial.println(degrees(AbsoluteAngle1), 4);
	Serial.print(F("Abs. Angle 2:\t"));
	Serial.println(degrees(AbsoluteAngle2), 4);
	Serial.print(F("Abs. Angle 3:\t"));
	Serial.println(degrees(AbsoluteAngle3), 4);

	Serial.print(F("Rel. Angle 12:\t"));
	Serial.println(degrees(RelativeAngle12), 4);
	Serial.print(F("Rel. Angle 23:\t"));
	Serial.println(degrees(RelativeAngle23), 4);
}


void setup()
{
	Serial.begin(115200);
	while (!Serial) { ; }

	L1 = 100; L2 = 50; L3 = 30;

	Serial.println(F("Solve Direct Relative"));
	SolveDirect(radians(55), radians(-20), radians(-70));
	Debug();

	Serial.println(F("Solve Inverse"));
	TargetX = 114.60289;
	TargetY = 36.62342;
	SolveReverse(radians(-70));
	Debug();

}


void loop()
{
	// Do nothing
	delay(10000);
}
```

```cpp
float L1, L2, D;
float AbsoluteAngle0, AbsoluteAngle1, AbsoluteAngle2;
float RelativeAngle12;
float TargetX, TargetY, TargetZ;

float Pythagoras(const float& x, const float& y)
{
	const float d = sqrt(x * x + y * y);
	return d;
}

float Pythagoras(const float& x, const float& y, const float& z)
{
	const float d = sqrt(x * x + y * y + z * z);
	return d;
}

float SolveInverseCosine(const float& l1, const float& l2, const float& l3)
{
	const float angle3 = acos((l1 * l1 + l2 * l2 - l3 * l3) / (2 * l1 * l2));

	return angle3;
}

float AbsoluteToRelative(float absolute1, float absolute2)
{
	return absolute2 - absolute1 + PI;
}

float RelativeToAbsolute(float relative12, float absolute1)
{
	return absolute1 + relative12 - PI;
}

void SolveTriangle(float& x, float& y, float& l1, float& l2, float& d, float& absoluteAngle1, float& absoluteAngle2, float& relativeAngle12)
{
	const float angle_0 = atan2(y, x);
	d = Pythagoras(x, y);

	const float angle_l1 = SolveInverseCosine(d, l2, l1);
	const float angle_l2 = SolveInverseCosine(d, l1, l2);
	const float angle_D = PI - angle_l1 - angle_l2;

	absoluteAngle1 = angle_0 + angle_l2;
	relativeAngle12 = angle_D;
	absoluteAngle2 = RelativeToAbsolute(relativeAngle12, absoluteAngle1);
}

void SolveDirect(float angle0, float angle1, float angle2)
{
	AbsoluteAngle0 = angle0;
	AbsoluteAngle1 = angle1;
	AbsoluteAngle2 = angle2;

	SolveDirect();
}

void SolveDirect()
{
	RelativeAngle12 = AbsoluteToRelative(AbsoluteAngle1, AbsoluteAngle2);

	float L = L1 * cos(AbsoluteAngle1) + L2 * cos(AbsoluteAngle2);
	TargetX = L * cos(AbsoluteAngle0);
	TargetY = L * sin(AbsoluteAngle0);
	TargetZ = L1 * sin(AbsoluteAngle1) + L2 * sin(AbsoluteAngle2);
	D = Pythagoras(TargetX, TargetY, TargetZ);
}

void SolveReverse()
{
	AbsoluteAngle0 = atan2(TargetY, TargetX);
	D = Pythagoras(TargetX, TargetY, TargetZ);

	float Lx = Pythagoras(TargetX, TargetY);
	float P2x = Lx;
	float P2y = TargetZ;

	float D0;
	SolveTriangle(P2x, P2y, L1, L2, D0, AbsoluteAngle1, AbsoluteAngle2, RelativeAngle12);
}

void Debug()
{
	Serial.print(F("Target X:\t"));
	Serial.println(TargetX, 4);
	Serial.print(F("Target Y:\t"));
	Serial.println(TargetY, 4);
	Serial.print(F("Target Z:\t"));
	Serial.println(TargetZ, 4);

	Serial.print(F("Abs. Angle 0:\t"));
	Serial.println(degrees(AbsoluteAngle0), 4);
	Serial.print(F("Abs. Angle 1:\t"));
	Serial.println(degrees(AbsoluteAngle1), 4);
	Serial.print(F("Abs. Angle 2:\t"));
	Serial.println(degrees(AbsoluteAngle2), 4);

	Serial.print(F("Rel. Angle 12:\t"));
	Serial.println(degrees(RelativeAngle12), 4);
}


void setup()
{
	Serial.begin(115200);
	while (!Serial) { ; }

	L1 = 100; L2 = 50;

	Serial.println(F("Solve Direct Relative"));
	SolveDirect(radians(30), radians(55), radians(-20));
	Debug();

	Serial.println(F("Solve Inverse"));
	TargetX = 90.3631;
	TargetY = 52.1711;
	TargetZ = 64.8142;
	SolveReverse();
	Debug();
}


void loop()
{
	// Do nothing
	delay(10000);
}
```

```cpp
float L1, L2, L3, D;
float AbsoluteAngle0, AbsoluteAngle1, AbsoluteAngle2, AbsoluteAngle3;
float RelativeAngle12, RelativeAngle23;
float TargetX, TargetY, TargetZ;

float Pythagoras(const float& x, const float& y)
{
	const float d = sqrt(x * x + y * y);
	return d;
}

float Pythagoras(const float& x, const float& y, const float& z)
{
	const float d = sqrt(x * x + y * y + z * z);
	return d;
}

float SolveInverseCosine(const float& l1, const float& l2, const float& l3)
{
	const float angle3 = acos((l1 * l1 + l2 * l2 - l3 * l3) / (2 * l1 * l2));

	return angle3;
}

float AbsoluteToRelative(float absolute1, float absolute2)
{
	return absolute2 - absolute1 + PI;
}

float RelativeToAbsolute(float relative12, float absolute1)
{
	return absolute1 + relative12 - PI;
}

void SolveTriangle(float& x, float& y, float& l1, float& l2, float& d, float& absoluteAngle1, float& absoluteAngle2, float& relativeAngle12)
{
	const float angle_0 = atan2(y, x);
	d = Pythagoras(x, y);

	const float angle_l1 = SolveInverseCosine(d, l2, l1);
	const float angle_l2 = SolveInverseCosine(d, l1, l2);
	const float angle_D = PI - angle_l1 - angle_l2;

	absoluteAngle1 = angle_0 + angle_l2;
	relativeAngle12 = angle_D;
	absoluteAngle2 = RelativeToAbsolute(relativeAngle12, absoluteAngle1);
}

void SolveDirect(float angle0, float angle1, float angle2, float angle3)
{
	AbsoluteAngle0 = angle0;
	AbsoluteAngle1 = angle1;
	AbsoluteAngle2 = angle2;
	AbsoluteAngle3 = angle3;

	SolveDirect();
}

void SolveDirect()
{
	RelativeAngle12 = AbsoluteToRelative(AbsoluteAngle1, AbsoluteAngle2);
	RelativeAngle23 = AbsoluteToRelative(AbsoluteAngle2, AbsoluteAngle3);

	float L = L1 * cos(AbsoluteAngle1) + L2 * cos(AbsoluteAngle2) + L3 * cos(AbsoluteAngle3);
	TargetX = L * cos(AbsoluteAngle0);
	TargetY = L * sin(AbsoluteAngle0);
	TargetZ = L1 * sin(AbsoluteAngle1) + L2 * sin(AbsoluteAngle2) + L3 * sin(AbsoluteAngle3);
	D = Pythagoras(TargetX, TargetY, TargetZ);
}

void SolveReverse(float absoluteAngle3)
{
	AbsoluteAngle3 = absoluteAngle3;
	AbsoluteAngle0 = atan2(TargetY, TargetX);
	D = Pythagoras(TargetX, TargetY, TargetZ);

	float Lx = Pythagoras(TargetX, TargetY);
	float P2x = Lx - L3 * cos(AbsoluteAngle3);
	float P2y = TargetZ - L3 * sin(AbsoluteAngle3);

	float D0;
	SolveTriangle(P2x, P2y, L1, L2, D0, AbsoluteAngle1, AbsoluteAngle2, RelativeAngle12);

	RelativeAngle23 = AbsoluteToRelative(AbsoluteAngle2, AbsoluteAngle3);
}

void Debug()
{
	Serial.print(F("Target X:\t"));
	Serial.println(TargetX, 4);
	Serial.print(F("Target Y:\t"));
	Serial.println(TargetY, 4);
	Serial.print(F("Target Z:\t"));
	Serial.println(TargetZ, 4);

	Serial.print(F("Abs. Angle 0:\t"));
	Serial.println(degrees(AbsoluteAngle0), 4);
	Serial.print(F("Abs. Angle 1:\t"));
	Serial.println(degrees(AbsoluteAngle1), 4);
	Serial.print(F("Abs. Angle 2:\t"));
	Serial.println(degrees(AbsoluteAngle2), 4);
	Serial.print(F("Abs. Angle 3:\t"));
	Serial.println(degrees(AbsoluteAngle3), 4);

	Serial.print(F("Rel. Angle 12:\t"));
	Serial.println(degrees(RelativeAngle12), 4);
	Serial.print(F("Rel. Angle 23:\t"));
	Serial.println(degrees(RelativeAngle23), 4);
}


void setup()
{
	Serial.begin(115200);
	while (!Serial)	{ ;	}

	L1 = 100; L2 = 50; L3 = 30;
	
	Serial.println(F("Solve Direct Relative"));
	SolveDirect(radians(30), radians(55), radians(-20), radians(-70));
	Debug();

	Serial.println(F("Solve Inverse"));
	TargetX = 99.2490;
	TargetY = 57.3014;
	TargetZ = 36.62342;
	SolveReverse(radians(-70));
	Debug();

}


void loop()
{
	// Do nothing
	delay(10000);
}
```