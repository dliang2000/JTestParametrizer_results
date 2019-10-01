# Uncertain Clone Group Candidates

# Commons_io_2.5

## 1

[183, 'src/test/java', 'org.apache.commons.io', 'FileSystemUtilsTestCase', 'testGetFreeSpaceUnix_String_EmptyPath', 289, 316]

[183, 'src/test/java', 'org.apache.commons.io', 'FileSystemUtilsTestCase', 'testGetFreeSpaceUnix_String_InvalidResponse1', 418, 444]

[183, 'src/test/java', 'org.apache.commons.io', 'FileSystemUtilsTestCase', 'testGetFreeSpaceUnix_String_InvalidResponse2', 446, 472]

[183, 'src/test/java', 'org.apache.commons.io', 'FileSystemUtilsTestCase', 'testGetFreeSpaceUnix_String_InvalidResponse3', 474, 500]

[183, 'src/test/java', 'org.apache.commons.io', 'FileSystemUtilsTestCase', 'testGetFreeSpaceUnix_String_InvalidResponse4', 502, 528]

```java
    @Test
    public void testGetFreeSpaceUnix_String_EmptyPath() throws Exception {
        final String lines =
                "Filesystem           1K-blocks      Used Available Use% Mounted on\n" +
                        "xxx:/home/users/s     14428928  12956424   1472504  90% /home/users/s";
        final FileSystemUtils fsu = new MockFileSystemUtils(0, lines);
        try {
            fsu.freeSpaceUnix("", false, false, -1);
            fail();
        } catch (final IllegalArgumentException ignore) {
        }
        try {
            fsu.freeSpaceUnix("", true, false, -1);
            fail();
        } catch (final IllegalArgumentException ignore) {
        }
        try {
            fsu.freeSpaceUnix("", true, true, -1);
            fail();
        } catch (final IllegalArgumentException ignore) {
        }
        try {
            fsu.freeSpaceUnix("", false, true, -1);
            fail();
        } catch (final IllegalArgumentException ignore) {
        }

    }

    @Test
    public void testGetFreeSpaceUnix_String_InvalidResponse1() throws Exception {
        final String lines =
                "Filesystem           1K-blocks      Used Available Use% Mounted on\n" +
                        "                      14428928  12956424       100";
        final FileSystemUtils fsu = new MockFileSystemUtils(0, lines);
        try {
            fsu.freeSpaceUnix("/home/users/s", false, false, -1);
            fail();
        } catch (final IOException ignore) {
        }
        try {
            fsu.freeSpaceUnix("/home/users/s", true, false, -1);
            fail();
        } catch (final IOException ignore) {
        }
        try {
            fsu.freeSpaceUnix("/home/users/s", false, true, -1);
            fail();
        } catch (final IOException ignore) {
        }
        try {
            fsu.freeSpaceUnix("/home/users/s", true, true, -1);
            fail();
        } catch (final IOException ignore) {
        }
    }

    @Test
    public void testGetFreeSpaceUnix_String_InvalidResponse2() throws Exception {
        final String lines =
                "Filesystem           1K-blocks      Used Available Use% Mounted on\n" +
                        "xxx:/home/users/s     14428928  12956424   nnnnnnn  90% /home/users/s";
        final FileSystemUtils fsu = new MockFileSystemUtils(0, lines);
        try {
            fsu.freeSpaceUnix("/home/users/s", false, false, -1);
            fail();
        } catch (final IOException ignore) {
        }
        try {
            fsu.freeSpaceUnix("/home/users/s", true, false, -1);
            fail();
        } catch (final IOException ignore) {
        }
        try {
            fsu.freeSpaceUnix("/home/users/s", false, true, -1);
            fail();
        } catch (final IOException ignore) {
        }
        try {
            fsu.freeSpaceUnix("/home/users/s", true, true, -1);
            fail();
        } catch (final IOException ignore) {
        }
    }

    @Test
    public void testGetFreeSpaceUnix_String_InvalidResponse3() throws Exception {
        final String lines =
                "Filesystem           1K-blocks      Used Available Use% Mounted on\n" +
                        "xxx:/home/users/s     14428928  12956424        -1  90% /home/users/s";
        final FileSystemUtils fsu = new MockFileSystemUtils(0, lines);
        try {
            fsu.freeSpaceUnix("/home/users/s", false, false, -1);
            fail();
        } catch (final IOException ignore) {
        }
        try {
            fsu.freeSpaceUnix("/home/users/s", true, false, -1);
            fail();
        } catch (final IOException ignore) {
        }
        try {
            fsu.freeSpaceUnix("/home/users/s", false, true, -1);
            fail();
        } catch (final IOException ignore) {
        }
        try {
            fsu.freeSpaceUnix("/home/users/s", true, true, -1);
            fail();
        } catch (final IOException ignore) {
        }
    }

    @Test
    public void testGetFreeSpaceUnix_String_InvalidResponse4() throws Exception {
        final String lines =
                "Filesystem           1K-blocks      Used Available Use% Mounted on\n" +
                        "xxx-yyyyyyy-zzz:/home/users/s";
        final FileSystemUtils fsu = new MockFileSystemUtils(0, lines);
        try {
            fsu.freeSpaceUnix("/home/users/s", false, false, -1);
            fail();
        } catch (final IOException ignore) {
        }
        try {
            fsu.freeSpaceUnix("/home/users/s", true, false, -1);
            fail();
        } catch (final IOException ignore) {
        }
        try {
            fsu.freeSpaceUnix("/home/users/s", false, true, -1);
            fail();
        } catch (final IOException ignore) {
        }
        try {
            fsu.freeSpaceUnix("/home/users/s", true, true, -1);
            fail();
        } catch (final IOException ignore) {
        }
    }
```

## 2


[566, 'src/test/java', 'org.apache.commons.io', 'FilenameUtilsTestCase', 'testGetPrefix', 609, 645]

[566, 'src/test/java', 'org.apache.commons.io', 'FilenameUtilsTestCase', 'testGetPath', 656, 691]

[566, 'src/test/java', 'org.apache.commons.io', 'FilenameUtilsTestCase', 'testGetPathNoEndSeparator', 699, 734]

```java
    @Test
    public void testGetPrefix() {
        assertEquals(null, FilenameUtils.getPrefix(null));
        assertEquals(null, FilenameUtils.getPrefix(":"));
        assertEquals(null, FilenameUtils.getPrefix("1:\\a\\b\\c.txt"));
        assertEquals(null, FilenameUtils.getPrefix("1:"));
        assertEquals(null, FilenameUtils.getPrefix("1:a"));
        assertEquals(null, FilenameUtils.getPrefix("\\\\\\a\\b\\c.txt"));
        assertEquals(null, FilenameUtils.getPrefix("\\\\a"));

        assertEquals("", FilenameUtils.getPrefix(""));
        assertEquals("\\", FilenameUtils.getPrefix("\\"));
        assertEquals("C:", FilenameUtils.getPrefix("C:"));
        assertEquals("C:\\", FilenameUtils.getPrefix("C:\\"));
        assertEquals("//server/", FilenameUtils.getPrefix("//server/"));
        assertEquals("~/", FilenameUtils.getPrefix("~"));
        assertEquals("~/", FilenameUtils.getPrefix("~/"));
        assertEquals("~user/", FilenameUtils.getPrefix("~user"));
        assertEquals("~user/", FilenameUtils.getPrefix("~user/"));

        assertEquals("", FilenameUtils.getPrefix("a\\b\\c.txt"));
        assertEquals("\\", FilenameUtils.getPrefix("\\a\\b\\c.txt"));
        assertEquals("C:\\", FilenameUtils.getPrefix("C:\\a\\b\\c.txt"));
        assertEquals("\\\\server\\", FilenameUtils.getPrefix("\\\\server\\a\\b\\c.txt"));

        assertEquals("", FilenameUtils.getPrefix("a/b/c.txt"));
        assertEquals("/", FilenameUtils.getPrefix("/a/b/c.txt"));
        assertEquals("C:/", FilenameUtils.getPrefix("C:/a/b/c.txt"));
        assertEquals("//server/", FilenameUtils.getPrefix("//server/a/b/c.txt"));
        assertEquals("~/", FilenameUtils.getPrefix("~/a/b/c.txt"));
        assertEquals("~user/", FilenameUtils.getPrefix("~user/a/b/c.txt"));

        assertEquals("", FilenameUtils.getPrefix("a\\b\\c.txt"));
        assertEquals("\\", FilenameUtils.getPrefix("\\a\\b\\c.txt"));
        assertEquals("~\\", FilenameUtils.getPrefix("~\\a\\b\\c.txt"));
        assertEquals("~user\\", FilenameUtils.getPrefix("~user\\a\\b\\c.txt"));
    }

    @Test
    public void testGetPath() {
        assertEquals(null, FilenameUtils.getPath(null));
        assertEquals("", FilenameUtils.getPath("noseperator.inthispath"));
        assertEquals("", FilenameUtils.getPath("/noseperator.inthispath"));
        assertEquals("", FilenameUtils.getPath("\\noseperator.inthispath"));
        assertEquals("a/b/", FilenameUtils.getPath("a/b/c.txt"));
        assertEquals("a/b/", FilenameUtils.getPath("a/b/c"));
        assertEquals("a/b/c/", FilenameUtils.getPath("a/b/c/"));
        assertEquals("a\\b\\", FilenameUtils.getPath("a\\b\\c"));

        assertEquals(null, FilenameUtils.getPath(":"));
        assertEquals(null, FilenameUtils.getPath("1:/a/b/c.txt"));
        assertEquals(null, FilenameUtils.getPath("1:"));
        assertEquals(null, FilenameUtils.getPath("1:a"));
        assertEquals(null, FilenameUtils.getPath("///a/b/c.txt"));
        assertEquals(null, FilenameUtils.getPath("//a"));

        assertEquals("", FilenameUtils.getPath(""));
        assertEquals("", FilenameUtils.getPath("C:"));
        assertEquals("", FilenameUtils.getPath("C:/"));
        assertEquals("", FilenameUtils.getPath("//server/"));
        assertEquals("", FilenameUtils.getPath("~"));
        assertEquals("", FilenameUtils.getPath("~/"));
        assertEquals("", FilenameUtils.getPath("~user"));
        assertEquals("", FilenameUtils.getPath("~user/"));

        assertEquals("a/b/", FilenameUtils.getPath("a/b/c.txt"));
        assertEquals("a/b/", FilenameUtils.getPath("/a/b/c.txt"));
        assertEquals("", FilenameUtils.getPath("C:a"));
        assertEquals("a/b/", FilenameUtils.getPath("C:a/b/c.txt"));
        assertEquals("a/b/", FilenameUtils.getPath("C:/a/b/c.txt"));
        assertEquals("a/b/", FilenameUtils.getPath("//server/a/b/c.txt"));
        assertEquals("a/b/", FilenameUtils.getPath("~/a/b/c.txt"));
        assertEquals("a/b/", FilenameUtils.getPath("~user/a/b/c.txt"));
    }

    @Test
    public void testGetPathNoEndSeparator() {
        assertEquals(null, FilenameUtils.getPath(null));
        assertEquals("", FilenameUtils.getPath("noseperator.inthispath"));
        assertEquals("", FilenameUtils.getPathNoEndSeparator("/noseperator.inthispath"));
        assertEquals("", FilenameUtils.getPathNoEndSeparator("\\noseperator.inthispath"));
        assertEquals("a/b", FilenameUtils.getPathNoEndSeparator("a/b/c.txt"));
        assertEquals("a/b", FilenameUtils.getPathNoEndSeparator("a/b/c"));
        assertEquals("a/b/c", FilenameUtils.getPathNoEndSeparator("a/b/c/"));
        assertEquals("a\\b", FilenameUtils.getPathNoEndSeparator("a\\b\\c"));

        assertEquals(null, FilenameUtils.getPathNoEndSeparator(":"));
        assertEquals(null, FilenameUtils.getPathNoEndSeparator("1:/a/b/c.txt"));
        assertEquals(null, FilenameUtils.getPathNoEndSeparator("1:"));
        assertEquals(null, FilenameUtils.getPathNoEndSeparator("1:a"));
        assertEquals(null, FilenameUtils.getPathNoEndSeparator("///a/b/c.txt"));
        assertEquals(null, FilenameUtils.getPathNoEndSeparator("//a"));

        assertEquals("", FilenameUtils.getPathNoEndSeparator(""));
        assertEquals("", FilenameUtils.getPathNoEndSeparator("C:"));
        assertEquals("", FilenameUtils.getPathNoEndSeparator("C:/"));
        assertEquals("", FilenameUtils.getPathNoEndSeparator("//server/"));
        assertEquals("", FilenameUtils.getPathNoEndSeparator("~"));
        assertEquals("", FilenameUtils.getPathNoEndSeparator("~/"));
        assertEquals("", FilenameUtils.getPathNoEndSeparator("~user"));
        assertEquals("", FilenameUtils.getPathNoEndSeparator("~user/"));

        assertEquals("a/b", FilenameUtils.getPathNoEndSeparator("a/b/c.txt"));
        assertEquals("a/b", FilenameUtils.getPathNoEndSeparator("/a/b/c.txt"));
        assertEquals("", FilenameUtils.getPathNoEndSeparator("C:a"));
        assertEquals("a/b", FilenameUtils.getPathNoEndSeparator("C:a/b/c.txt"));
        assertEquals("a/b", FilenameUtils.getPathNoEndSeparator("C:/a/b/c.txt"));
        assertEquals("a/b", FilenameUtils.getPathNoEndSeparator("//server/a/b/c.txt"));
        assertEquals("a/b", FilenameUtils.getPathNoEndSeparator("~/a/b/c.txt"));
        assertEquals("a/b", FilenameUtils.getPathNoEndSeparator("~user/a/b/c.txt"));
    }
```

# Solr_6.6.6

## 3

[2968, 'src/test', 'org.apache.solr.cloud', 'OverriddenZkACLAndCredentialsProvidersTest', 'testNoCredentialsSolrZkClientFactoryUsingVMParamsProvidersButWithDifferentVMParamsNames', 161, 173]

[2968, 'src/test', 'org.apache.solr.cloud', 'OverriddenZkACLAndCredentialsProvidersTest', 'testWrongCredentialsSolrZkClientFactoryUsingVMParamsProvidersButWithDifferentVMParamsNames', 175, 187]

[2968, 'src/test', 'org.apache.solr.cloud', 'OverriddenZkACLAndCredentialsProvidersTest', 'testAllCredentialsSolrZkClientFactoryUsingVMParamsProvidersButWithDifferentVMParamsNames', 189, 201]

[2968, 'src/test', 'org.apache.solr.cloud', 'OverriddenZkACLAndCredentialsProvidersTest', 'testReadonlyCredentialsSolrZkClientFactoryUsingVMParamsProvidersButWithDifferentVMParamsNames', 203, 215]

```java
  @Test
  public void testNoCredentialsSolrZkClientFactoryUsingVMParamsProvidersButWithDifferentVMParamsNames() throws Exception {
    useNoCredentials();
    
    SolrZkClient zkClient = new SolrZkClientUsingVMParamsProvidersButWithDifferentVMParamsNames(zkServer.getZkAddress(), AbstractZkTestCase.TIMEOUT);
    try {
      VMParamsZkACLAndCredentialsProvidersTest.doTest(zkClient,
          false, false, false, false, false,
          false, false, false, false, false);
    } finally {
      zkClient.close();
    }
  }

  @Test
  public void testWrongCredentialsSolrZkClientFactoryUsingVMParamsProvidersButWithDifferentVMParamsNames() throws Exception {
    useWrongCredentials();
    
    SolrZkClient zkClient = new SolrZkClientUsingVMParamsProvidersButWithDifferentVMParamsNames(zkServer.getZkAddress(), AbstractZkTestCase.TIMEOUT);
    try {
      VMParamsZkACLAndCredentialsProvidersTest.doTest(zkClient,
          false, false, false, false, false,
          false, false, false, false, false);
    } finally {
      zkClient.close();
    }
  }

  @Test
  public void testAllCredentialsSolrZkClientFactoryUsingVMParamsProvidersButWithDifferentVMParamsNames() throws Exception {
    useAllCredentials();
    
    SolrZkClient zkClient = new SolrZkClientUsingVMParamsProvidersButWithDifferentVMParamsNames(zkServer.getZkAddress(), AbstractZkTestCase.TIMEOUT);
    try {
      VMParamsZkACLAndCredentialsProvidersTest.doTest(zkClient,
          true, true, true, true, true,
          true, true, true, true, true);
    } finally {
      zkClient.close();
    }
  }

  @Test
  public void testReadonlyCredentialsSolrZkClientFactoryUsingVMParamsProvidersButWithDifferentVMParamsNames() throws Exception {
    useReadonlyCredentials();
    
    SolrZkClient zkClient = new SolrZkClientUsingVMParamsProvidersButWithDifferentVMParamsNames(zkServer.getZkAddress(), AbstractZkTestCase.TIMEOUT);
    try {
      VMParamsZkACLAndCredentialsProvidersTest.doTest(zkClient,
          true, true, false, false, false,
          false, false, false, false, false);
    } finally {
      zkClient.close();
    }
  }
```

# joda_time_2.10

## 4

[481, 'src/test/java', 'org.joda.time.format', 'TestDateTimeFormatter', 'testParseLocalDate_weekyear_month_week_2010', 452, 456]

[481, 'src/test/java', 'org.joda.time.format', 'TestDateTimeFormatter', 'testParseLocalDate_weekyear_month_week_2011', 458, 462]

[481, 'src/test/java', 'org.joda.time.format', 'TestDateTimeFormatter', 'testParseLocalDate_weekyear_month_week_2012', 464, 468]

[481, 'src/test/java', 'org.joda.time.format', 'TestDateTimeFormatter', 'testParseLocalDate_year_month_week_2010', 478, 482]

[481, 'src/test/java', 'org.joda.time.format', 'TestDateTimeFormatter', 'testParseLocalDate_year_month_week_2011', 484, 488]

[481, 'src/test/java', 'org.joda.time.format', 'TestDateTimeFormatter', 'testParseLocalDate_year_month_week_2012', 490, 494]

[481, 'src/test/java', 'org.joda.time.format', 'TestDateTimeFormatter', 'testParseLocalDate_year_month_week_2013', 496, 500]

[481, 'src/test/java', 'org.joda.time.format', 'TestDateTimeFormatter', 'testParseLocalDate_year_month_week_2014', 502, 506]

[481, 'src/test/java', 'org.joda.time.format', 'TestDateTimeFormatter', 'testParseLocalDate_year_month_week_2015', 508, 512]

[481, 'src/test/java', 'org.joda.time.format', 'TestDateTimeFormatter', 'testParseLocalDate_year_month_week_2016', 514, 518]

```java
public void testParseLocalDate_weekyear_month_week_2010() {
    Chronology chrono = GJChronology.getInstanceUTC();
    DateTimeFormatter f = DateTimeFormat.forPattern("xxxx-MM-ww").withChronology(chrono);
    assertEquals(new LocalDate(2010, 1, 4, chrono), f.parseLocalDate("2010-01-01"));
}

public void testParseLocalDate_weekyear_month_week_2011() {
    Chronology chrono = GJChronology.getInstanceUTC();
    DateTimeFormatter f = DateTimeFormat.forPattern("xxxx-MM-ww").withChronology(chrono);
    assertEquals(new LocalDate(2011, 1, 3, chrono), f.parseLocalDate("2011-01-01"));
}

public void testParseLocalDate_weekyear_month_week_2012() {
    Chronology chrono = GJChronology.getInstanceUTC();
    DateTimeFormatter f = DateTimeFormat.forPattern("xxxx-MM-ww").withChronology(chrono);
    assertEquals(new LocalDate(2012, 1, 2, chrono), f.parseLocalDate("2012-01-01"));
}

public void testParseLocalDate_year_month_week_2010() {
    Chronology chrono = GJChronology.getInstanceUTC();
    DateTimeFormatter f = DateTimeFormat.forPattern("yyyy-MM-ww").withChronology(chrono);
    assertEquals(new LocalDate(2010, 1, 4, chrono), f.parseLocalDate("2010-01-01"));
}

public void testParseLocalDate_year_month_week_2011() {
    Chronology chrono = GJChronology.getInstanceUTC();
    DateTimeFormatter f = DateTimeFormat.forPattern("yyyy-MM-ww").withChronology(chrono);
    assertEquals(new LocalDate(2011, 1, 3, chrono), f.parseLocalDate("2011-01-01"));
}

public void testParseLocalDate_year_month_week_2012() {
    Chronology chrono = GJChronology.getInstanceUTC();
    DateTimeFormatter f = DateTimeFormat.forPattern("yyyy-MM-ww").withChronology(chrono);
    assertEquals(new LocalDate(2012, 1, 2, chrono), f.parseLocalDate("2012-01-01"));
}

public void testParseLocalDate_year_month_week_2013() {
    Chronology chrono = GJChronology.getInstanceUTC();
    DateTimeFormatter f = DateTimeFormat.forPattern("yyyy-MM-ww").withChronology(chrono);
    assertEquals(new LocalDate(2012, 12, 31, chrono), f.parseLocalDate("2013-01-01"));  // 2013-01-01 would be better, but this is OK
}

public void testParseLocalDate_year_month_week_2014() {
    Chronology chrono = GJChronology.getInstanceUTC();
    DateTimeFormatter f = DateTimeFormat.forPattern("yyyy-MM-ww").withChronology(chrono);
    assertEquals(new LocalDate(2013, 12, 30, chrono), f.parseLocalDate("2014-01-01"));  // 2014-01-01 would be better, but this is OK
}

public void testParseLocalDate_year_month_week_2015() {
    Chronology chrono = GJChronology.getInstanceUTC();
    DateTimeFormatter f = DateTimeFormat.forPattern("yyyy-MM-ww").withChronology(chrono);
    assertEquals(new LocalDate(2014, 12, 29, chrono), f.parseLocalDate("2015-01-01"));  // 2015-01-01 would be better, but this is OK
}

public void testParseLocalDate_year_month_week_2016() {
    Chronology chrono = GJChronology.getInstanceUTC();
    DateTimeFormatter f = DateTimeFormat.forPattern("yyyy-MM-ww").withChronology(chrono);
    assertEquals(new LocalDate(2016, 1, 4, chrono), f.parseLocalDate("2016-01-01"));
}
```