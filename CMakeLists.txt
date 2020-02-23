cmake_minimum_required (VERSION 3.8)

project(HandmadeHero)

set(CMAKE_BUILD_TYPE RelWithDebInfo)
set(HANDMADE_DIR ${CMAKE_CURRENT_LIST_DIR}/handmade/)
set(SourceDir ${HANDMADE_DIR}code/)
set(DataDir ${HANDMADE_DIR}data/)
set(CTimeDir ${HANDMADE_DIR}ctime/)
set(BuildDir ${CMAKE_CURRENT_LIST_DIR}/build/)
set(CMAKE_RUNTIME_OUTPUT_DIRECTORY ${BuildDir}) 
set(CMAKE_LIBRARY_OUTPUT_DIRECTORY ${BuildDir}) 
set(CMAKE_ARCHIVE_OUTPUT_DIRECTORY ${BuildDir}) 

list(APPEND CommonCompileFlags -diagnostics:column -WL -O2 -nologo -fp:fast -fp:except- -Gm- -GR- -EHa- -Zo -Oi -WX -W4 -wd4201 -wd4100 -wd4189 -wd4505 -wd4127 -FC -Z7 -GS- -Gs9999999)
list(APPEND CommonCompileDefinitions HANDMADE_INTERNAL=1 HANDMADE_SLOW=1 HANDMADE_WIN3=1)
list(APPEND CommonLinkerFlags -STACK:0x100000,0x100000 -incremental:no -opt:ref)
list(APPEND CommonLinkerLibs user32.lib gdi32.lib winmm.lib kernel32.lib)
list(APPEND UtilsDefinitions ${CommonCompileDefinitions} _CRT_SECURE_NO_WARNINGS)

include_directories(${SourceDir})
add_compile_options(${CommonCompileFlags})

# PNG parser
add_executable(PNGParser ${SourceDir}test_png.cpp)
set_target_properties(PNGParser PROPERTIES OUTPUT_NAME test_png)
target_link_libraries(PNGParser PRIVATE ${CommonLinkerLibs})
target_link_options(PNGParser PRIVATE ${CommonLinkerFlags})
target_compile_definitions(PNGParser PRIVATE ${UtilsDefinitions})

# Sampling generator
add_executable(SamplingGenerator ${SourceDir}hhsphere.cpp)
set_target_properties(SamplingGenerator PROPERTIES OUTPUT_NAME hhsphere)
target_link_libraries(SamplingGenerator PRIVATE ${CommonLinkerLibs})
target_link_options(SamplingGenerator PRIVATE ${CommonLinkerFlags})
target_compile_definitions(SamplingGenerator PRIVATE ${UtilsDefinitions})

# Win32 font extractor
add_executable(FontExtractor ${SourceDir}hhfont.cpp)
set_target_properties(FontExtractor PROPERTIES OUTPUT_NAME hhfont)
target_link_libraries(FontExtractor PRIVATE ${CommonLinkerLibs})
target_link_options(FontExtractor PRIVATE ${CommonLinkerFlags})
target_compile_definitions(FontExtractor PRIVATE ${UtilsDefinitions})

# Asset file editor
add_executable(AssetEditor ${SourceDir}hhaedit.cpp)
set_target_properties(AssetEditor PROPERTIES OUTPUT_NAME hhaedit)
target_link_libraries(AssetEditor PRIVATE ${CommonLinkerLibs})
target_link_options(AssetEditor PRIVATE ${CommonLinkerFlags})
target_compile_definitions(AssetEditor PRIVATE ${UtilsDefinitions})


# Renderers
add_library(OpenGLRenderer SHARED ${SourceDir}win32_handmade_opengl.cpp)
set_target_properties(OpenGLRenderer PROPERTIES OUTPUT_NAME win32_handmade_opengl)
target_compile_definitions(OpenGLRenderer PRIVATE ${CommonCompileDefinitions})
target_compile_options(OpenGLRenderer PRIVATE -MTd)
target_link_libraries(OpenGLRenderer PRIVATE gdi32.lib opengl32.lib user32.lib)
target_link_options(OpenGLRenderer PRIVATE -EXPORT:Win32LoadRenderer -EXPORT:Win32BeginFrame -EXPORT:Win32EndFrame)
add_custom_command(TARGET OpenGLRenderer WORKING_DIRECTORY ${BuildDir}
	PRE_BUILD 
		COMMAND del ARGS win32_handmade_opengl_*.pdb 
		COMMAND echo ARGS WAITING FOR PDB > lock.tmp
)
add_custom_command(TARGET OpenGLRenderer WORKING_DIRECTORY ${BuildDir}
	POST_BUILD 
		COMMAND ren ARGS win32_handmade_opengl.pdb win32_handmade_opengl_%random%.pdb 
)

# Game
add_library(GameLib SHARED ${SourceDir}handmade.cpp ${SourceDir}handmade_msvc.c)
set_target_properties(GameLib PROPERTIES OUTPUT_NAME handmade)
target_compile_options(GameLib PRIVATE -MTd -Fmhandmade.map)
target_compile_definitions(GameLib PRIVATE ${CommonCompileDefinitions})
target_link_options(GameLib PRIVATE ${CommonLinkerFlags} -EXPORT:GameGetSoundSamples -EXPORT:GameUpdateAndRender -EXPORT:DEBUGGameFrameEnd)
add_custom_command(TARGET GameLib WORKING_DIRECTORY ${BuildDir}
	PRE_BUILD 
		COMMAND "${CTimeDir}ctime.exe" ARGS -begin handmade_hero.ctm
		COMMAND set ARGS LastError=%ERRORLEVEL%
		COMMAND ${CMAKE_COMMAND} -E make_directory ${BuildDir}
		COMMAND ${CMAKE_COMMAND} -E make_directory ${DataDir}
		COMMAND del ARGS handmade_*.pdb 
)
add_custom_command(TARGET GameLib WORKING_DIRECTORY ${BuildDir}
	POST_BUILD 
		COMMAND ren ARGS handmade.pdb handmade_%random%.pdb 
		COMMAND "${CTimeDir}ctime.exe" ARGS -end handmade_hero.ctm %LastError%
		COMMAND del ARGS lock.tmp
)

add_executable(Game WIN32 ${SourceDir}win32_handmade.cpp ${SourceDir}handmade_msvc.c)
set_target_properties(Game PROPERTIES OUTPUT_NAME win32_handmade)
target_compile_options(Game PRIVATE -Fmwin32_handmade.map)
target_compile_definitions(Game PRIVATE ${CommonCompileDefinitions})
target_link_libraries(Game PRIVATE ${CommonLinkerLibs})
target_link_options(Game PRIVATE ${CommonLinkerFlags} -NODEFAULTLIB)

# Renderer testbed
add_executable(RenderTest WIN32 ${SourceDir}win32_renderer_test.cpp)
set_target_properties(RenderTest PROPERTIES OUTPUT_NAME win32_renderer_test)
target_compile_options(RenderTest PRIVATE -MTd)
target_compile_definitions(RenderTest PRIVATE ${CommonCompileDefinitions})
target_link_libraries(RenderTest PRIVATE ${CommonLinkerLibs})
target_link_options(RenderTest PRIVATE ${CommonLinkerFlags})