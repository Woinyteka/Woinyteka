Experiment Parameter Class Implementation:
import java.util.Iterator;
import java.util.NoSuchElementException;

public class ExperimentParameter implements Iterable {
private double startValue;
private double endValue;
private double step;
private boolean isRange;

public static class ParseError extends Exception {
    public ParseError(String message) {
        super(message);
    }
}

// Constructor that initializes the class from a string
public ExperimentParameter(String parameter) throws ParseError {
    try {
        if (parameter.contains(":")) {
            String[] parts = parameter.split(":");
            if (parts.length != 3) {
                throw new ParseError("Invalid range format.");
            }
            startValue = Double.parseDouble(parts[0]);
            endValue = Double.parseDouble(parts[1]);
            step = Double.parseDouble(parts[2]);

            if (step == 0) {
                throw new ParseError("Step value cannot be zero.");
            }

            isRange = true;
        } else {
            startValue = Double.parseDouble(parameter);
            endValue = startValue;
            step = 0;
            isRange = false;
        }
    } catch (NumberFormatException e) {
        throw new ParseError("Invalid number format.");
    }
}

// Getter methods
public double getStartValue() {
    return startValue;
}

public double getEndValue() {
    return endValue;
}

public double getStep() {
    return step;
}

public boolean isRange() {
    return isRange;
}

// Iterable method to iterate over the range
@Override
public Iterator<Double> iterator() {
    return new Iterator<Double>() {
        private double currentValue = startValue;
        private boolean hasNext = isRange ? step > 0 ? startValue <= endValue : startValue >= endValue : true;

        @Override
        public boolean hasNext() {
            return hasNext;
        }

        @Override
        public Double next() {
            if (!hasNext) {
                throw new NoSuchElementException();
            }

            double value = currentValue;

            if (isRange) {
                currentValue += step;

                if ((step > 0 && currentValue > endValue) || (step < 0 && currentValue < endValue)) {
                    hasNext = false;
                }
            } else {
                hasNext = false;
            }

            return value;
        }
    };
}

// Main method for testing
public static void main(String[] args) {
    try {
        ExperimentParameter singleParam = new ExperimentParameter("5");
        System.out.println("Single value parameter:");
        for (double value : singleParam) {
            System.out.println(value);
        }

        ExperimentParameter rangeParam = new ExperimentParameter("1:10:2");
        System.out.println("Range parameter:");
        for (double value : rangeParam) {
            System.out.println(value);
        }

        ExperimentParameter invalidParam = new ExperimentParameter("1:10");
    } catch (ParseError e) {
        System.err.println("Parse error: " + e.getMessage());
    }
}
}- 👋 Hi, I’m @Woinyteka
- 👀 I’m interested in ...
- 🌱 I’m currently learning ...
- 💞️ I’m looking to collaborate on ...
- 📫 How to reach me ...
- 😄 Pronouns: ...
- ⚡ Fun fact: ...

<!---
Woinyteka/Woinyteka is a ✨ special ✨ repository because its `README.md` (this file) appears on your GitHub profile.
You can click the Preview link to take a look at your changes.
--->
