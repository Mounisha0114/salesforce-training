public with sharing class ApplicationService {

    public static void validateApplications(List<Application__c> applications) {

        Set<Id> studentIds = new Set<Id>();
        Set<Id> jobIds = new Set<Id>();

        // Collect Student and Job IDs
        for (Application__c app : applications) {

            if (app.Student__c == null) {
                app.addError('Student is required.');
                continue;
            }

            if (app.Job__c == null) {
                app.addError('Job is required.');
                continue;
            }

            studentIds.add(app.Student__c);
            jobIds.add(app.Job__c);
        }

        // Query Students
        Map<Id, Student__c> studentMap = new Map<Id, Student__c>([
            SELECT Id,
                   CGPA__c,
                   Active_Backlogs__c,
                   Department__c
            FROM Student__c
            WHERE Id IN :studentIds
        ]);

        // Query Jobs
        Map<Id, Job__c> jobMap = new Map<Id, Job__c>([
            SELECT Id,
                   Minimum_Gpa__c,
                   Allowed_Backlogs_c__c,
                   Eligible_Department__c
            FROM Job__c
            WHERE Id IN :jobIds
        ]);

        // Validate Applications
        for (Application__c app : applications) {

            Student__c student = studentMap.get(app.Student__c);
            Job__c job = jobMap.get(app.Job__c);

            if (student == null || job == null) {
                continue;
            }

            // CGPA Validation
            if (student.CGPA__c < job.Minimum_Gpa__c) {
                app.addError('Student CGPA is below the required minimum.');
            }
            
            System.debug('Student Backlogs = ' + student.Active_Backlogs__c);
			System.debug('Job Allowed Backlogs = ' + job.Allowed_Backlogs_c__c);


            // Backlog Validation
            if (student.Active_Backlogs__c > job.Allowed_Backlogs_c__c) {
                app.addError('Student has more backlogs than allowed.');
            }

            // Department Validation
            if (!String.isBlank(job.Eligible_Department__c)
                && !job.Eligible_Department__c.contains(student.Department__c)) {

                app.addError('Student department is not eligible for this job.');
            }
        }
    }
}
