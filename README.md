# proyecto
arduino

bool AsyncTask::Update()
{
	if (_isActive == false) return false;

	if (static_cast<unsigned long>(micros() - _startTime) >= Interval)
	{
		if (OnFinish != nullptr) OnFinish();

		_isExpired = !AutoReset;
		_isActive = AutoReset;
		Reset();
	}
	return _isExpired;
}

void AsyncTask::Update(AsyncTask &next)
{
	if (Update())
	{
		_isExpired = false;
		next.Start();
	}
}

void AsyncTask::SetIntervalMillis(unsigned long interval)
{
	Interval = 1000 * interval;
}

void AsyncTask::SetIntervalMicros(unsigned long interval)
{
	Interval = interval;
}

unsigned long AsyncTask::GetStartTime()
{
	return _startTime;
}

unsigned long AsyncTask::GetElapsedTime()
{
	return micros() - _startTime;
}

unsigned long AsyncTask::GetRemainingTime()
{
	return interval - micros() + _startTime;
}

bool AsyncTask::IsActive() const
{
	return _isActive;
