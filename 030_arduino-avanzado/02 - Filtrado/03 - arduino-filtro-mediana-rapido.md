> Códigos de ejemplo de los tutoriales de www.luisllamas.es
>
> Enlace entrada: https://www.luisllamas.es/arduino-filtro-mediana-rapido/
>
> Todo el contenido distribuido bajo licencia CCC, salvo indicación expresa

## Filtro mediana con Quick Sort
```cpp
const int windowSize = 3;
int buffer[windowSize];
int medianBuffer[windowSize];
int* medianBufferAccessor = medianBuffer;


#define MEDIAN(a, n) a[(((n)&1)?((n)/2):(((n)/2)-1))];

int values[] = { 7729, 7330, 10075, 10998, 11502, 11781, 12413, 12530, 14070, 13789, 18186, 14401, 16691, 16654, 17424, 21104, 17230, 20656, 21584, 21297, 19986, 20808, 19455, 24029, 21455, 21350, 19854, 23476, 19349, 16996, 20546, 17187, 15548, 9179, 8586, 7095, 9718, 5148, 4047, 3873, 4398, 2989, 3848, 2916, 1142, 2427, 250, 2995, 1918, 4297, 617, 2715, 1662, 1621, 960, 500, 2114, 2354, 2900, 4878, 8972, 9460, 11283, 16147, 16617, 16778, 18711, 22036, 28432, 29756, 24944, 27199, 27760, 30706, 31671, 32185, 32290, 30470, 32616, 32075, 32210, 28822, 30823, 29632, 29157, 31585, 24133, 23245, 22516, 18513, 18330, 15450, 12685, 11451, 11280, 9116, 7975, 8263, 8203, 4641, 5232, 5724, 4347, 4319, 3045, 1099, 2035, 2411, 1727, 852, 1134, 966, 2838, 6033, 2319, 3294, 3587, 9076, 5194, 6725, 6032, 6444, 10293, 9507, 10881, 11036, 12789, 12813, 14893, 16465, 16336, 16854, 19249, 23126, 21461, 18657, 20474, 24871, 20046, 22832, 21681, 21978, 23053, 20569, 24801, 19045, 20092, 19470, 18446, 18851, 18210, 15078, 16309, 15055, 14427, 15074, 10776, 14319, 14183, 7984, 8344, 7071, 9675, 5985, 3679, 2321, 6757, 3291, 5003, 1401, 1724, 1857, 2605, 803, 2742, 2971, 2306, 3722, 3332, 4427, 5762, 5383, 7692, 8436, 13660, 8018, 9303, 10626, 16171, 14163, 17161, 19214, 21171, 17274, 20616, 18281, 21171, 18220, 19315, 22558, 21393, 22431, 20186, 24619, 21997, 23938, 20029, 20694, 20648, 21173, 20377, 19147, 18578, 16839, 15735, 15907, 18059, 12111, 12178, 11201, 10577, 11160, 8485, 7065, 7852, 5865, 4856, 3955, 6803, 3444, 1616, 717, 3105, 704, 1473, 1948, 4534, 5800, 1757, 1038, 2435, 4677, 8155, 6870, 4611, 5372, 6304, 7868, 10336, 9091 };
int valuesLength = sizeof(values) / sizeof(int);

int getMeasure()
{
   int static index = 0;
   index++;
   return values[index - 1];
}

int appendToBuffer(int value)
{
   *medianBufferAccessor = value;
   medianBufferAccessor++;
   if (medianBufferAccessor >= medianBuffer + windowSize)
      medianBufferAccessor = medianBuffer;
}

int elementCount;
float AddValue(int value)
{
   appendToBuffer(value);

   if (elementCount < windowSize)
      ++elementCount;
}

void setup()
{
   Serial.begin(115200);

   float timeMean = 0;
   for (int iCount = 0; iCount < valuesLength; iCount++)
   {
      int value = getMeasure();   
      long timeCount = micros();

      AddValue(value);
      memcpy(buffer, medianBuffer, sizeof(medianBuffer));
      QuickSortAsc(buffer, 0, elementCount - 1);
      int med = MEDIAN(medianBuffer, windowSize);
      
      timeCount = micros() - timeCount;
      timeMean += timeCount;
      Serial.print(value);
      Serial.print(",");
      Serial.println(med);
   }

   Serial.println(timeMean / valuesLength);
}

void loop()
{
}

void QuickSortAsc(int* arr, const int left, const int right)
{
   int i = left, j = right;
   int tmp;

   int pivot = arr[(left + right) / 2];
   while (i <= j)
   {
      while (arr[i]<pivot) i++;
      while (arr[j]>pivot) j--;
      if (i <= j)
      {
         tmp = arr[i];
         arr[i] = arr[j];
         arr[j] = tmp;
         i++;
         j--;
      }
   };

   if (left<j)
      QuickSortAsc(arr, left, j);
   if (i<right)
      QuickSortAsc(arr, i, right);
}
```


## Filtro mediana con Quick Median
```cpp
const int windowSize = 33;
int buffer[windowSize];
int medianBuffer[windowSize];
int* medianBufferAccessor = medianBuffer;


#define ELEM_SWAP(a,b) { int t=(a);(a)=(b);(b)=t; }

#define MEDIAN(a,n) kth_smallest(a,n,(((n)&1)?((n)/2):(((n)/2)-1)))

int values[] = { 7729, 7330, 10075, 10998, 11502, 11781, 12413, 12530, 14070, 13789, 18186, 14401, 16691, 16654, 17424, 21104, 17230, 20656, 21584, 21297, 19986, 20808, 19455, 24029, 21455, 21350, 19854, 23476, 19349, 16996, 20546, 17187, 15548, 9179, 8586, 7095, 9718, 5148, 4047, 3873, 4398, 2989, 3848, 2916, 1142, 2427, 250, 2995, 1918, 4297, 617, 2715, 1662, 1621, 960, 500, 2114, 2354, 2900, 4878, 8972, 9460, 11283, 16147, 16617, 16778, 18711, 22036, 28432, 29756, 24944, 27199, 27760, 30706, 31671, 32185, 32290, 30470, 32616, 32075, 32210, 28822, 30823, 29632, 29157, 31585, 24133, 23245, 22516, 18513, 18330, 15450, 12685, 11451, 11280, 9116, 7975, 8263, 8203, 4641, 5232, 5724, 4347, 4319, 3045, 1099, 2035, 2411, 1727, 852, 1134, 966, 2838, 6033, 2319, 3294, 3587, 9076, 5194, 6725, 6032, 6444, 10293, 9507, 10881, 11036, 12789, 12813, 14893, 16465, 16336, 16854, 19249, 23126, 21461, 18657, 20474, 24871, 20046, 22832, 21681, 21978, 23053, 20569, 24801, 19045, 20092, 19470, 18446, 18851, 18210, 15078, 16309, 15055, 14427, 15074, 10776, 14319, 14183, 7984, 8344, 7071, 9675, 5985, 3679, 2321, 6757, 3291, 5003, 1401, 1724, 1857, 2605, 803, 2742, 2971, 2306, 3722, 3332, 4427, 5762, 5383, 7692, 8436, 13660, 8018, 9303, 10626, 16171, 14163, 17161, 19214, 21171, 17274, 20616, 18281, 21171, 18220, 19315, 22558, 21393, 22431, 20186, 24619, 21997, 23938, 20029, 20694, 20648, 21173, 20377, 19147, 18578, 16839, 15735, 15907, 18059, 12111, 12178, 11201, 10577, 11160, 8485, 7065, 7852, 5865, 4856, 3955, 6803, 3444, 1616, 717, 3105, 704, 1473, 1948, 4534, 5800, 1757, 1038, 2435, 4677, 8155, 6870, 4611, 5372, 6304, 7868, 10336, 9091 };
int valuesLength = sizeof(values) / sizeof(int);

int getMeasure()
{
   int static index = 0;
   index++;
   return values[index - 1];
}

int appendToBuffer(int value)
{
   *medianBufferAccessor = value;
   medianBufferAccessor++;
   if (medianBufferAccessor >= medianBuffer + windowSize)
      medianBufferAccessor = medianBuffer;
}

int elementCount;
float AddValue(int value)
{
   appendToBuffer(value);

   if (elementCount < windowSize)
      ++elementCount;
}

void setup()
{
   Serial.begin(115200);

   float timeMean = 0;
   for (int iCount = 0; iCount < valuesLength; iCount++)
   {
      int value = getMeasure();   
      long timeCount = micros();

      AddValue(value);
      memcpy(buffer, medianBuffer, sizeof(medianBuffer));
      int med = MEDIAN(medianBuffer, elementCount);
      timeCount = micros() - timeCount;
      timeMean += timeCount;
      Serial.print(value);
      Serial.print(",");
      Serial.println(med);
   }

   Serial.println(timeMean / valuesLength);
}

void loop()
{
}

int kth_smallest(int a[], int n, int k)
{
   int i, j, l, m;
   int x;
   l = 0;
   m = n - 1;
   while (l<m)
   {
      x = a[k];
      i = l;
      j = m;
      do {
         while (a[i]<x) i++;
         while (x<a[j]) j--;
         if (i <= j)
         {
            ELEM_SWAP(a[i], a[j]);
            i++;
            j--;
         }
      } while (i <= j);
      if (j<k) l = i;
      if (k<i) m = j;
   }
   return a[k];
}
```


## Filtro mediana Quick Median Filter
```cpp
#define MEDIAN_FILTER_WINDOW 7

int values[] = { 7729, 7330, 10075, 10998, 11502, 11781, 12413, 12530, 14070, 13789, 18186, 14401, 16691, 16654, 17424, 21104, 17230, 20656, 21584, 21297, 19986, 20808, 19455, 24029, 21455, 21350, 19854, 23476, 19349, 16996, 20546, 17187, 15548, 9179, 8586, 7095, 9718, 5148, 4047, 3873, 4398, 2989, 3848, 2916, 1142, 2427, 250, 2995, 1918, 4297, 617, 2715, 1662, 1621, 960, 500, 2114, 2354, 2900, 4878, 8972, 9460, 11283, 16147, 16617, 16778, 18711, 22036, 28432, 29756, 24944, 27199, 27760, 30706, 31671, 32185, 32290, 30470, 32616, 32075, 32210, 28822, 30823, 29632, 29157, 31585, 24133, 23245, 22516, 18513, 18330, 15450, 12685, 11451, 11280, 9116, 7975, 8263, 8203, 4641, 5232, 5724, 4347, 4319, 3045, 1099, 2035, 2411, 1727, 852, 1134, 966, 2838, 6033, 2319, 3294, 3587, 9076, 5194, 6725, 6032, 6444, 10293, 9507, 10881, 11036, 12789, 12813, 14893, 16465, 16336, 16854, 19249, 23126, 21461, 18657, 20474, 24871, 20046, 22832, 21681, 21978, 23053, 20569, 24801, 19045, 20092, 19470, 18446, 18851, 18210, 15078, 16309, 15055, 14427, 15074, 10776, 14319, 14183, 7984, 8344, 7071, 9675, 5985, 3679, 2321, 6757, 3291, 5003, 1401, 1724, 1857, 2605, 803, 2742, 2971, 2306, 3722, 3332, 4427, 5762, 5383, 7692, 8436, 13660, 8018, 9303, 10626, 16171, 14163, 17161, 19214, 21171, 17274, 20616, 18281, 21171, 18220, 19315, 22558, 21393, 22431, 20186, 24619, 21997, 23938, 20029, 20694, 20648, 21173, 20377, 19147, 18578, 16839, 15735, 15907, 18059, 12111, 12178, 11201, 10577, 11160, 8485, 7065, 7852, 5865, 4856, 3955, 6803, 3444, 1616, 717, 3105, 704, 1473, 1948, 4534, 5800, 1757, 1038, 2435, 4677, 8155, 6870, 4611, 5372, 6304, 7868, 10336, 9091 };
int valuesLength = sizeof(values) / sizeof(int);

int getMeasure()
{
   int static index = 0;
   index++;
   return values[index - 1];
}

void setup()
{
   Serial.begin(115200);

   float timeMean = 0;
   for (int iCount = 0; iCount < valuesLength; iCount++)
   {

      int value = getMeasure();
      long timeCount = micros();

      int med = medianFilter(value);
      timeCount = micros() - timeCount;
      timeMean += timeCount;
      Serial.print(values[iCount]);
      Serial.print(",");
      Serial.println(med);
   }

   Serial.println(timeMean / valuesLength);
}

void loop()
{
}


#define STOPPER 0
uint16_t medianFilter(uint16_t value)
{
   struct node
   {
      struct node *next;
      uint16_t value;
   };
   static struct node buffer[MEDIAN_FILTER_WINDOW] = { 0 };
   static struct node *iterator = buffer;
   static struct node smaller = { nullptr, STOPPER };
   static struct node bigger = { &smaller, 0 };

   struct node *successor;
   struct node *accessor;
   struct node *accessorPrev;
   struct node *median;
   uint16_t i;

   if (value == STOPPER)
      value++;

   if ((++iterator - buffer) >= MEDIAN_FILTER_WINDOW)
      iterator = buffer;

   iterator->value = value;
   successor = iterator->next;
   median = &bigger;
   accessorPrev = nullptr;
   accessor = &bigger;

   if (accessor->next == iterator)
      accessor->next = successor;
   accessorPrev = accessor;
   accessor = accessor->next;

   for (i = 0; i < MEDIAN_FILTER_WINDOW; ++i)
   {
      if (accessor->next == iterator)
         accessor->next = successor;

      if (accessor->value < value)
      {
         iterator->next = accessorPrev->next;
         accessorPrev->next = iterator;
         value = STOPPER;
      };

      median = median->next;
      if (accessor == &smaller)
         break;
      accessorPrev = accessor;
      accessor = accessor->next;

      if (accessor->next == iterator)
         accessor->next = successor;

      if (accessor->value < value)
      {
         iterator->next = accessorPrev->next;
         accessorPrev->next = iterator;
         value = STOPPER;
      }

      if (accessor == &smaller)
         break;
      accessorPrev = accessor;
      accessor = accessor->next;
   }
   return median->value;
}
```


## Bonus: Filtro mediana de tamaño 3
```cpp
const int windowSize = 3;
int medianBuffer[windowSize];
int* medianBufferAccessor = medianBuffer;

int values[] = { 7729, 7330, 10075, 10998, 11502, 11781, 12413, 12530, 14070, 13789, 18186, 14401, 16691, 16654, 17424, 21104, 17230, 20656, 21584, 21297, 19986, 20808, 19455, 24029, 21455, 21350, 19854, 23476, 19349, 16996, 20546, 17187, 15548, 9179, 8586, 7095, 9718, 5148, 4047, 3873, 4398, 2989, 3848, 2916, 1142, 2427, 250, 2995, 1918, 4297, 617, 2715, 1662, 1621, 960, 500, 2114, 2354, 2900, 4878, 8972, 9460, 11283, 16147, 16617, 16778, 18711, 22036, 28432, 29756, 24944, 27199, 27760, 30706, 31671, 32185, 32290, 30470, 32616, 32075, 32210, 28822, 30823, 29632, 29157, 31585, 24133, 23245, 22516, 18513, 18330, 15450, 12685, 11451, 11280, 9116, 7975, 8263, 8203, 4641, 5232, 5724, 4347, 4319, 3045, 1099, 2035, 2411, 1727, 852, 1134, 966, 2838, 6033, 2319, 3294, 3587, 9076, 5194, 6725, 6032, 6444, 10293, 9507, 10881, 11036, 12789, 12813, 14893, 16465, 16336, 16854, 19249, 23126, 21461, 18657, 20474, 24871, 20046, 22832, 21681, 21978, 23053, 20569, 24801, 19045, 20092, 19470, 18446, 18851, 18210, 15078, 16309, 15055, 14427, 15074, 10776, 14319, 14183, 7984, 8344, 7071, 9675, 5985, 3679, 2321, 6757, 3291, 5003, 1401, 1724, 1857, 2605, 803, 2742, 2971, 2306, 3722, 3332, 4427, 5762, 5383, 7692, 8436, 13660, 8018, 9303, 10626, 16171, 14163, 17161, 19214, 21171, 17274, 20616, 18281, 21171, 18220, 19315, 22558, 21393, 22431, 20186, 24619, 21997, 23938, 20029, 20694, 20648, 21173, 20377, 19147, 18578, 16839, 15735, 15907, 18059, 12111, 12178, 11201, 10577, 11160, 8485, 7065, 7852, 5865, 4856, 3955, 6803, 3444, 1616, 717, 3105, 704, 1473, 1948, 4534, 5800, 1757, 1038, 2435, 4677, 8155, 6870, 4611, 5372, 6304, 7868, 10336, 9091 };
int valuesLength = sizeof(values) / sizeof(int);

int getMeasure()
{
   int static index = 0;
   index++;
   return values[index - 1];
}

int appendToBuffer(int value)
{
   *medianBufferAccessor = value;
   medianBufferAccessor++;
   if (medianBufferAccessor >= medianBuffer + windowSize)
      medianBufferAccessor = medianBuffer;
}

float AddValue(int value)
{
   appendToBuffer(value);
}

void setup()
{
   Serial.begin(115200);

   float timeMean = 0;
   for (int iCount = 0; iCount < valuesLength; iCount++)
   {
      int value = getMeasure();
      long timeCount = micros();

      AddValue(value);
      int med = median3(medianBuffer);

      timeCount = micros() - timeCount;
      timeMean += timeCount;
      Serial.print(value);
      Serial.print(",");
      Serial.println(med);
   }

   Serial.println(timeMean / valuesLength);
}

void loop()
{
}

int median3(int arr[])
{
   if ((arr[0] <= arr[1]) && (arr[0] <= arr[2]))
      return (arr[1] <= arr[2]) ? arr[1] : arr[2];
   else if ((arr[1] <= arr[0]) && (arr[1] <= arr[2]))
      return (arr[0] <= arr[2]) ? arr[0] : arr[2];
   else
      return (arr[0] <= arr[1]) ? arr[0] : arr[1];
}
```


