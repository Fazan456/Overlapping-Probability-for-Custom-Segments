# Probability of Overlap Between Custom Segments

## Goal

The goal of this project is to calculate the probability of overlap between custom segments in a particular area on specific date & time. The code processes input from a CSV file to determine how segments intersect based on MAID (Mobile Advertising ID) data.

## Modules

- **os**: Handles file and folder operations, and system commands.
- **pandas**: Facilitates data handling and analysis.
- **psycopg2**: Connects to PostgreSQL databases, particularly Amazon Redshift, for querying and data management.

## Functions

### 1. `calling_redshift()`

**Purpose**: Connects to Amazon Redshift using `psycopg2`.

**Returns**: A connection object to Amazon Redshift.

### 2. `retrieve_maid_for_segments(start_date, end_date, min_lat, min_long, max_lat, max_long, start_hour, end_hour, country_iso3, segment_file)`

**Purpose**: Retrieves MAIDs for each segment within a specified geographical region and time frame, saving the results to separate CSV files for further analysis.

**Parameters**:

- `start_date`: Start date of the time frame (e.g., '2024-02-22').
- `end_date`: End date of the time frame (e.g., '2024-02-23').
- `min_lat`: Minimum latitude of the region (e.g., -6.2303).
- `min_long`: Minimum longitude of the region (e.g., 106.8042).
- `max_lat`: Maximum latitude of the region (e.g., -6.2213).
- `max_long`: Maximum longitude of the region (e.g., 106.8132).
- `start_hour`: Start hour of the time frame (e.g., 0).
- `end_hour`: End hour of the time frame (e.g., 23).
- `country_iso3`: ISO 3166-1 alpha-3 country code (e.g., "IDN" for Indonesia).
- `segment_file`: Path to the CSV file containing segment names.

**Returns**: A DataFrame containing segment names and corresponding MAIDs.

### 3. `calc_prob(df, output_dir)`

**Purpose**: Calculates the probability of overlap between segments and writes the results to a CSV file.

**Parameters**:

- `df`: DataFrame containing segment names and corresponding MAIDs.
- `output_dir`: Directory where the output CSV file will be saved.

**Output**: A CSV file containing segment combinations and their overlap probabilities.

**Error Handling**: Manages potential `ZeroDivisionError` exceptions during probability calculations.

## Main Code Execution

1. **Parameters Initialization**: Define geographical boundaries, time frame, country code, and segment file path.
2. **Retrieving Data**: Call `retrieve_maid_for_segments()` to get MAIDs for segments within the specified criteria.
3. **Calculating Probabilities**: Pass the retrieved data to `calc_prob()` to compute overlap probabilities.
4. **Saving Results**: Save the results to CSV files with clear naming conventions indicating the parameters used.

## Sample Calculation (Optional)

To further understand the overlap probability calculation:

**Formula**:
\[ \text{overlapping\_Probability} = \frac{\left(\frac{\text{Intersection of MAIDs in both segments}}{\text{Total unique MAID in Segment 1}}\right) + \left(\frac{\text{Intersection of MAIDs in both segments}}{\text{Total unique MAID in Segment 2}}\right)}{2} \]

**Example**:

- **Segment 1**: 350 unique MAIDs
- **Segment 2**: 400 unique MAIDs
- **Intersection of MAIDs**: 200

**Calculation**:
\[ \text{overlapping\_Probability} = \frac{\left(\frac{200}{350}\right) + \left(\frac{200}{400}\right)}{2} \]
\[ \text{overlapping\_Probability} = \frac{0.571 + 0.5}{2} \]
\[ \text{overlapping\_Probability} = 0.5355 \]

