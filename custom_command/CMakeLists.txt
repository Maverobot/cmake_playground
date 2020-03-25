# Credit: https://gist.github.com/baiwfg2/39881ba703e9c74e95366ed422641609

cmake_minimum_required(VERSION 2.6)
project(custom_command)

set(TEST_FILE "log.txt")

# add_custom_command does not create a new target. You have to define targets
# explicitly by add_executable, add_library or add_custom_target in order to
# make them visible to make
add_custom_command(
  OUTPUT ${TEST_FILE}
  COMMAND
    ls > ${TEST_FILE}
    # Display the given message before the commands are executed at build time
  COMMENT "Creating ${TEST_FILE}")

# target zoo is always built
add_custom_target(
  zoo ALL
  COMMAND
    echo "This is ALL target 'zoo', and it depends on ${TEST_FILE}"
  COMMAND
    cat "${TEST_FILE}"
     # If the file exists, then commands related to that file won't be executed
    # DONOT let other target depends on the same OUTPUT as current target, or it
    # may be bad when doing parallel make
  DEPENDS ${TEST_FILE}
  VERBATIM)
