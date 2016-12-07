---
---


StarEngine管理各种Manager
{		
	FontManager::GetInstance()->EraseFonts();
	DebugDraw::DeleteSingleton();
	ScaleSystem::DeleteSingleton();
	FontManager::DeleteSingleton();
	SpriteAnimationManager::DeleteSingleton();
	TextureManager::DeleteSingleton();
	GraphicsManager::DeleteSingleton();
	SpriteBatch::DeleteSingleton();
	AudioManager::DeleteSingleton();
	PathFindManager::DeleteSingleton();
	SceneManager::DeleteSingleton();
	Logger::DeleteSingleton();
	TimeManager::DeleteSingleton();
}


Context主要提供dt

基本类型：

Entity{ setName getName}


Scene{
	Object{
		many Components	
	}		
}




