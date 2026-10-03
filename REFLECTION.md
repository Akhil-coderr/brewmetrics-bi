# Reflection

This project showed me that building a Business Intelligence solution is not only about creating visuals in Power BI, but also about maintaining a clear and traceable development process.

## AI-Assisted Development & Testing

Copilot was useful during the project, especially when generating DAX ideas and helping structure the project documentation. However, the suggestions could not be accepted without testing. 

During the DAX development process, the `RANKX` measure initially used `ALLSELECTED`, which did not produce the expected ranking in the visual. After testing the result in Power BI, the measure was corrected to use `ALL`, which produced the required product ranking. This showed me that Copilot can provide a useful starting point, but the generated solution must be checked against the actual data model and visual context.

## Version Control & Git Workflow

The Git workflow also changed how I approached the project compared with a normal single-file lab. Instead of making all changes at once, I committed the following as separate steps:
- Star schema creation
- DAX measures development
- Dashboard visual design
- Documentation updates

This structured workflow made the development process easier to trace and allowed each major change to be reviewed through the commit history.

## Conclusion

Overall, the project helped me understand the connection between Power BI development, DAX, AI-assisted development, and version control.
