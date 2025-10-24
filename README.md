# Agriculture-Dashboard---AWS-to-Power-BI-Pipeline
![Power BI](https://img.shields.io/badge/Power%20BI-F2C811?style=for-the-badge&logo=powerbi&logoColor=black)
![Snowflake](https://img.shields.io/badge/Snowflake-29B5E8?style=for-the-badge&logo=snowflake&logoColor=white)
![AWS](https://img.shields.io/badge/AWS-232F3E?style=for-the-badge&logo=amazon-aws&logoColor=white)

## 📊 Project Overview

An end-to-end data engineering and analytics solution that integrates AWS S3, Snowflake, and Power BI to create an interactive agriculture dashboard. This project demonstrates cloud data pipeline architecture, storage integration, and business intelligence visualization.

## 🏗️ Architecture

```
AWS S3 (Data Source) → Snowflake (Data Warehouse) → Power BI (Visualization)
```

### Components:
- **AWS S3**: Cloud storage for raw agriculture dataset
- **AWS IAM**: Role-based access control for secure data access
- **Snowflake**: Cloud data warehouse for data transformation and management
- **Power BI**: Interactive dashboard for data visualization and insights

## 🚀 Features

- ✅ Secure data integration using AWS IAM roles
- ✅ External stage configuration in Snowflake
- ✅ Data transformation and categorization
- ✅ Year-based grouping (Y1: 2004-2009, Y2: 2010-2015, Y3: 2016-2019)
- ✅ Rainfall categorization (Low, Medium, High)
- ✅ Interactive Power BI dashboard
- ✅ Real-time data refresh capabilities

## 📁 Project Structure

```
├── data/
│   └── AGRICULTURE DASHBOARD USING SNOWFLAKES.pbix
├── sql/
│   └── snowflake_setup.sql
├── docs/
│   ├── architecture_diagram.png
│   └── dashboard_screenshots/
├── config/
│   └── aws_iam_policy.json
└── README.md
```

## 🔧 Setup Instructions

### Prerequisites
- AWS Account with S3 access
- Snowflake Account
- Power BI Desktop
- Basic knowledge of SQL and cloud services

### Step 1: AWS Configuration

1. Create an S3 bucket and upload your agriculture dataset
2. Create an IAM role with the following policy:

```json
{
    "Version": "2012-10-17",
    "Statement": [
        {
            "Effect": "Allow",
            "Action": [
                "s3:GetObject",
                "s3:ListBucket"
            ],
            "Resource": [
                "arn:aws:s3:::your-bucket-name/*",
                "arn:aws:s3:::your-bucket-name"
            ]
        }
    ]
}
```

3. Note down the Role ARN for Snowflake integration

### Step 2: Snowflake Setup

1. Create storage integration:

```sql
CREATE OR REPLACE STORAGE INTEGRATION PBI_Integration
    TYPE = EXTERNAL_STAGE
    STORAGE_PROVIDER = 'S3'
    ENABLED = TRUE
    STORAGE_AWS_ROLE_ARN = 'arn:aws:iam::605941533351:role/powerbi.role'
    STORAGE_ALLOWED_LOCATIONS = ('s3://powerbi_project/')
    COMMENT = 'Integration for Power BI project';
```

2. Create database and schema:

```sql
CREATE DATABASE PowerBI;
CREATE SCHEMA PBI_Data;
```

3. Create table structure:

```sql
CREATE TABLE PBI_Dataset (
    Year int,
    Location string,
    Area int,
    Rainfall float,
    Temperature float,
    Soil_type string,
    Irrigation string,
    Yelds int,
    Humidity float,
    Crops string,
    Price int,
    Season string
);
```

4. Create external stage and load data:

```sql
CREATE STAGE PowerBI.PBI_Data.pbi_stage
    URL = 's3://project.test.files'
    STORAGE_INTEGRATION = PBI_Integration;

COPY INTO PBI_Dataset
FROM @pbi_stage
FILE_FORMAT = (type=csv field_delimiter=',' skip_header=1)
ON_ERROR = 'continue';
```

### Step 3: Data Transformation

1. Add Year Groups:

```sql
ALTER TABLE agriculture
ADD Year_Group String;

UPDATE agriculture
SET Year_Group = 'Y1'
WHERE year >= 2004 AND year <= 2009;

UPDATE agriculture
SET Year_Group = 'Y2'
WHERE year >= 2010 AND year <= 2015;

UPDATE agriculture
SET Year_Group = 'Y3'
WHERE year >= 2016 AND year <= 2019;
```

2. Add Rainfall Categories:

```sql
ALTER TABLE agriculture
ADD rainfall_groups string;

UPDATE AGRICULTURE
SET RAINFALL_GROUPS = 'LOW'
WHERE RAINFALL >= 255 AND RAINFALL < 1200;

UPDATE AGRICULTURE
SET RAINFALL_GROUPS = 'MEDIUM'
WHERE RAINFALL >= 1200 AND RAINFALL < 2800;

UPDATE AGRICULTURE
SET RAINFALL_GROUPS = 'HIGH'
WHERE RAINFALL >= 2800;
```

### Step 4: Power BI Connection

1. Open Power BI Desktop
2. Get Data → Snowflake
3. Enter Snowflake server details
4. Authenticate and select the `PBI_Dataset` table
5. Load data and create visualizations

## 📊 Dashboard Features

The Power BI dashboard includes:

- **Tempreture Analysis**: Year-wise crop yield trends
- **Rainfall Impact**: Correlation between rainfall groups and crop performance
- **Soil Analysis**: Soil type impact on crop yields
- **Humidity Analysis**: Humidity impact on Year Group, Location, Crop , Seasons
- **Interactive Filters**: Dynamic filtering by year group, location, and crop type

## 🎯 Key Insights

- Identified optimal rainfall ranges for different crops
- Year-over-year crop yield trends (2004-2019)
- Regional agricultural performance patterns
- Soil type effectiveness analysis
- Seasonal crop recommendations

## 🔐 Security Best Practices

- IAM roles used instead of access keys
- Least privilege principle applied
- Storage integration for secure S3 access
- No hardcoded credentials in code
- Regular access audit recommended

## 📈 Future Enhancements

- [ ] Automated data refresh scheduling
- [ ] Machine learning predictions for crop yields
- [ ] Weather API integration for real-time updates
- [ ] Mobile-responsive dashboard version
- [ ] Additional data sources integration
- [ ] Alerting system for abnormal patterns

## 🤝 Contributing

Contributions are welcome! Please feel free to submit a Pull Request.

## 📝 License

This project is licensed under the MIT License - see the LICENSE file for details.

## 👤 Author

**Shejal Agarwal**
- LinkedIn: [www.linkedin.com/in/shejal-agarwal2314]
- GitHub: [Shejal]

## 🙏 Acknowledgments

- Agriculture dataset source
- AWS documentation
- Snowflake community
- Power BI community

## 📧 Contact

For questions or feedback, please reach out via [shejalagarwal1234@gmail.com]

---

⭐ If you found this project helpful, please give it a star!
